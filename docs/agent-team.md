# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate the work of this custom
agent team for Mona's Project Pulse dashboard:

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 5.5 (copilot) | Coordinates the specialists, turns the plan into scoped phases, manages dependencies and parallel work, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |
| **Planner** | Claude Opus 5.5 (copilot) | Researches the repository and relevant documentation, then produces an implementation plan with steps, file assignments, dependencies, risks, edge cases, and validation expectations. | `.github/agents/planner.agent.md` |
| **Coder** | GPT-5.5 (copilot) | Implements application logic and assigned runnable-app support, following repository patterns and validating changes. | `.github/agents/coder.agent.md` |
| **Designer** | Gemini 3.7 Flash (copilot) | Shapes the dashboard's user experience, accessibility, information hierarchy, responsive behavior, and visual design. | `.github/agents/designer.agent.md` |
