# Agent team

Summary of the custom agent team for Mona's Project Pulse dashboard.

- Orchestrator — Model: Claude Opus 4.7 (copilot). Responsibility: coordinate Planner, Coder, and Designer; decompose requests into phases, assign explicit file scopes, manage parallelism and integration checks. Definition: .github/agents/orchestrator.agent.md

- Planner — Model: Claude Opus 4.7 (copilot). Responsibility: research repository, produce ordered implementation steps, file assignments, dependencies, and validation expectations. Definition: .github/agents/planner.agent.md

- Coder — Model: GPT-5.5 (copilot). Responsibility: implement code changes, create/run support configuration for runnable apps (Project Pulse-specific launch settings), validate behavior and tests. Definition: .github/agents/coder.agent.md

- Designer — Model: Gemini 3.1 Pro (copilot). Responsibility: UX/UI, accessibility, visual design, and responsive dashboard styling (project cards, badges, CSS hooks). Definition: .github/agents/designer.agent.md

Note: Work is orchestrated via GitHub Copilot CLI running inside a Codespace; the learner controls git operations (staging/committing/pushing) through Copilot CLI prompts.
