# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate the custom agent team building Mona's Project Pulse dashboard.

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 4.7 (copilot) | Coordinates the dashboard effort, breaks requests into phases, delegates work to specialists with explicit file scopes, manages dependencies, and verifies that the integrated result works together. | `.github/agents/orchestrator.agent.md` |
| **Planner** | Claude Opus 4.7 (copilot) | Researches the repository and relevant documentation, identifies requirements, dependencies, edge cases, and risks, and produces an ordered implementation plan without writing code. | `.github/agents/planner.agent.md` |
| **Coder** | GPT-5.5 (copilot) | Implements the dashboard logic and runnable-app support within the assigned scope, keeping behavior explicit, deterministic, testable, and validated. | `.github/agents/coder.agent.md` |
| **Designer** | Gemini 3.1 Pro (copilot) | Defines and implements the Project Pulse user experience, including information hierarchy, accessibility, responsive behavior, visual clarity, project cards, status badges, priority treatment, and dashboard styling. | `.github/agents/designer.agent.md` |
