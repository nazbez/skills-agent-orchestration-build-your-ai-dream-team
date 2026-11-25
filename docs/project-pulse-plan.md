# Project Pulse Implementation Plan

## 1. Project Pulse Goal

Create a lightweight, polished static dashboard for Mona’s team that helps contributors quickly scan:

- Active projects
- Project owners
- Project status
- Recent activity
- Priority or risk level
- Contributor-friendly summaries

The implementation must use `app/index.html`, `app/styles.css`, `app/project-data.json`, and a VS Code launch configuration named **Run Project Pulse Dashboard**.

## 2. Agent Responsibilities

### Orchestrator

- Convert this plan into ordered, scoped phases.
- Assign only explicit file scopes.
- Ensure the Planner, Designer, and Coder remain isolated where necessary.
- Integrate outputs and verify the final dashboard.
- Do not stage, commit, or push changes.

### Planner

- Establish the implementation contract and file ownership.
- Define the project-data shape and HTML/CSS hooks.
- Identify dependencies, parallel work, edge cases, and validation expectations.
- Produce this plan and do not implement application files.

### Designer

- Own `app/styles.css`.
- Create polished project cards, status badges, priority treatments, readable spacing, and responsive layouts.
- Ensure accessible contrast, keyboard-visible interactions, semantic compatibility, and visual hierarchy.
- Use deterministic hooks including `.dashboard` and `.project-card`.
- Report design decisions and validation recommendations.
- Do not modify HTML, JSON, or launch configuration.

### Coder

- Implement `app/index.html`, including data loading and dynamic project rendering.
- Create `app/project-data.json` with a top-level `projects` array.
- Create `.vscode/launch.json`.
- Use strict JSON for launch and data configuration.
- Keep behavior deterministic and add clear empty/error handling.
- Validate implementation behavior and report remaining risks.
- Do not modify `app/styles.css`.

## 3. Ordered Implementation Phases

### Phase 1: Establish the implementation contract

**Owner:** Planner with Orchestrator coordination

**Actions:**

- Confirm required project fields: `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Add an optional `summary` field to individual project objects when needed to satisfy the dashboard goal.
- Define stable HTML hooks: `.dashboard`, `.project-card`, and status and priority badge classes.
- Establish the loading, empty, and error behavior for `app/index.html`.

**Files:** No implementation files; planning only.

**Exit criteria:** Every specialist receives an explicit scope and shared data/schema contract.

### Phase 2: Create project data

**Owner:** Coder

**Actions:**

- Create `app/project-data.json`.
- Use a top-level `projects` array.
- Include varied project examples with descriptive owners, statuses, activities, priorities, and summaries.
- Keep values suitable for status and priority badge styling.

**File assignment:** `app/project-data.json`

**Dependencies:** Phase 1 contract.

### Phase 3: Implement the dashboard HTML

**Owner:** Coder

**Actions:**

- Create `app/index.html`.
- Add semantic dashboard structure.
- Fetch and render `app/project-data.json`.
- Dynamically create project cards using `.project-card`.
- Include visible project name, owner, status, recent activity, priority, and summary.
- Preserve deterministic hooks and ensure the JavaScript is compatible with static serving.
- Handle fetch failures and empty project lists with clear user-facing messages.

**File assignment:** `app/index.html`

**Dependencies:** Phase 1 contract and the schema from Phase 2.

### Phase 4: Design and implement dashboard styles

**Owner:** Designer

**Actions:**

- Create `app/styles.css`.
- Style `.dashboard` and `.project-card`.
- Add status badges, priority treatments, readable typography, spacing, shadows, and rounded corners.
- Create a responsive card/grid layout.
- Maintain accessible color contrast and visible keyboard-focus behavior.
- Ensure the first view clearly resembles a polished Project Pulse dashboard.

**File assignment:** `app/styles.css`

**Dependencies:** The HTML hooks defined in Phase 1. This phase may run concurrently with Phase 3 because file scopes do not overlap.

### Phase 5: Create the VS Code launch configuration

**Owner:** Coder

**Actions:**

- Create `.vscode/launch.json`.
- Add a strict-JSON configuration named **Run Project Pulse Dashboard**.
- Set `cwd` to `${workspaceFolder}/app`.
- Serve the `app/` directory, not the repository root.
- Open `http://localhost:4173/index.html` or another documented deterministic port.
- Confirm the launch flow opens the dashboard rather than a directory listing.

**File assignment:** `.vscode/launch.json`

**Dependencies:** The app directory and `app/index.html` must exist.

### Phase 6: Integrate and validate

**Owner:** Orchestrator, with Coder and Designer validation support

**Actions:**

- Inspect the integrated files for syntax and scope violations.
- Serve the app from `app/`.
- Verify launch behavior, dashboard rendering, responsive layout, and accessibility.
- Fix only defects directly caused by integration.
- Produce a concise completion report listing changed files and validation results.

**File assignments:** No unrestricted edits. Coder fixes HTML, JSON, or launch issues; Designer fixes only CSS.

## 4. File Ownership

| File | Primary owner | Allowed responsibilities |
| --- | --- | --- |
| `app/index.html` | Coder | Structure, data fetching, rendering, loading/error states |
| `app/styles.css` | Designer | Layout, typography, cards, badges, responsiveness, accessibility |
| `app/project-data.json` | Coder | Top-level `projects` array and project records |
| `.vscode/launch.json` | Coder | VS Code launch name, app working directory, server command, dashboard URL |

No specialist should modify a file outside this table.

## 5. Dependencies

1. The HTML implementation depends on the agreed project-data schema.
2. The CSS implementation depends on the HTML hook contract.
3. The launch configuration depends on the existence of `app/index.html`.
4. Final validation depends on HTML, CSS, JSON, and launch configuration integration.

## 6. Parallel Work Decisions

### Safe to run in parallel

- Coder creates `app/project-data.json`.
- Coder creates `app/index.html` after the schema contract is fixed.
- Designer creates `app/styles.css` using the same fixed HTML hook contract.

HTML and CSS may proceed concurrently because their file scopes do not overlap.

### Must run sequentially

- JSON schema contract precedes HTML rendering logic.
- HTML and CSS hook contracts precede final styling.
- Launch configuration follows creation of the app files.
- Integrated validation follows implementation.
- Fix cycles follow validation findings.

## 7. Validation Expectations

### Data validation

- `app/project-data.json` parses as strict JSON.
- The root object contains a `projects` array.
- Every project includes `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Projects include summaries or comparable contributor context.

### HTML validation

- `app/index.html` loads successfully from a static server.
- Project cards render for all valid project records.
- `fetch("./project-data.json")` succeeds.
- `.dashboard` and `.project-card` hooks appear deterministically.
- Empty and fetch-error states are explicit and understandable.
- The browser console contains no JavaScript or resource-loading errors.

### CSS validation

- Cards, badges, priority treatments, and spacing are visibly styled.
- The dashboard is readable on desktop and narrow/mobile widths.
- Text and status colors meet practical accessibility contrast.
- Layout remains usable without horizontal clipping.
- Interactive or focusable elements expose visible keyboard focus.

### Launch validation

- `.vscode/launch.json` parses as strict JSON.
- The configuration is named **Run Project Pulse Dashboard**.
- The launch working directory is `${workspaceFolder}/app`.
- The launch serves `app/`, not the repository root.
- The opened URL ends at `index.html`.
- No directory listing replaces the dashboard.

### Integration validation

- HTML, CSS, and JSON load together.
- No launch, data, or styling change introduces unrelated repository modifications.
- Validation reports only the changed Project Pulse files and the required launch behavior.

## 8. Edge Cases

- Empty `projects` array: show an informative empty state.
- Invalid or unreachable JSON: show a concise error instead of blank output.
- Missing optional summary: omit or render an explicit fallback without breaking cards.
- Unknown status or priority values: render as neutral labels instead of failing.
- Long project names or activity text: maintain wrapping and prevent layout overflow.
- Narrow viewports: retain readable cards and non-overlapping badges.
- Static-server path differences: use relative asset and data paths from `app/index.html`.

## 9. Open Questions

- None. The implementation contract is sufficiently specified by the Project Pulse brief and custom-agent definitions.
