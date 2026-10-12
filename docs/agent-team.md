# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate the custom agent team building Mona's Project Pulse dashboard.

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 4.7 (copilot) | Coordinates the specialists, divides the work into phases with clear file ownership, delegates tasks, and verifies the integrated result. It does not implement the dashboard itself. | `.github/agents/orchestrator.agent.md` |
| **Planner** | Claude Opus 4.7 (copilot) | Researches the repository, dependencies, documentation, and edge cases, then provides an ordered, actionable implementation plan with dependencies and validation expectations. | `.github/agents/planner.agent.md` |
| **Designer** | Gemini 3.1 Pro (copilot) | Shapes the dashboard experience, including usability, accessibility, information hierarchy, responsive behavior, and polished Project Pulse styling. | `.github/agents/designer.agent.md` |
| **Coder** | GPT-5.5 (copilot) | Implements assigned application logic and runnable-app support, following repository patterns and validating the changes. | `.github/agents/coder.agent.md` |

## How the team works together
The Orchestrator starts with the Planner's research, then assigns design and implementation work according to the plan, keeping overlapping file scopes in separate phases.