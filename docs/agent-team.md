# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate the custom agent team building Mona's Project Pulse dashboard.

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | gpt-5.6-luna | Coordinates the specialists, divides the work into phases with clear file ownership, delegates tasks, and verifies the integrated result. It does not implement the dashboard itself. | `.github/agents/orchestrator.agent.md` |
| **Planner** | gpt-5.6-luna | Researches the repository, dependencies, documentation, and edge cases, then provides an ordered, actionable implementation plan with dependencies and validation expectations. | `.github/agents/planner.agent.md` |
| **Designer** | gpt-5.6-luna | Shapes the dashboard experience, including usability, accessibility, information hierarchy, responsive behavior, and polished Project Pulse styling. | `.github/agents/designer.agent.md` |
| **Coder** | gpt-5.6-luna | Implements assigned application logic and runnable-app support, following repository patterns and validating the changes. | `.github/agents/coder.agent.md` |

## How the team works together
The Orchestrator starts with the Planner's research, then assigns design and implementation work according to the plan, keeping overlapping file scopes in separate phases.