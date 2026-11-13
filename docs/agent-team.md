# Agent team for Mona's Project Pulse dashboard

We will use a small custom agent team from the repository's `.github/agents/` folder, orchestrated through GitHub Copilot CLI in a Codespace.

- Planner — Target model: Claude Opus 5.5 (copilot). Responsibility: research the repository, read the relevant files, identify requirements and edge cases, and produce a phased implementation plan with dependencies and validation expectations. Definition: `.github/agents/planner.agent.md`.
- Orchestrator — Target model: Claude Opus 5.5 (copilot). Responsibility: break the plan into phases, delegate work to the specialist agents, keep file ownership clear, and verify that the integrated work hangs together without implementing code directly. Definition: `.github/agents/orchestrator.agent.md`.
- Coder — Target model: GPT-5.5 (copilot). Responsibility: implement the dashboard logic, fix issues, keep the code structured and testable, and create any app support config required for a runnable Project Pulse experience. Definition: `.github/agents/coder.agent.md`.
- Designer — Target model: Gemini 3.7 Flash (copilot). Responsibility: shape the UI/UX and visual design of the Project Pulse dashboard, with focus on accessibility, information hierarchy, responsive layout, and polished dashboard styling. Definition: `.github/agents/designer.agent.md`.

This team gives us a clear split between planning, orchestration, implementation, and design while keeping the entire workflow coordinated through GitHub Copilot CLI in the Codespace environment.
