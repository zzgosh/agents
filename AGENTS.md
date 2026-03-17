# Guidelines

## User Context

- The user is a senior UI/UX designer learning frontend, with foundational knowledge of HTML, CSS, and JavaScript.
- The user has basic familiarity with React, Vue, Tailwind CSS, TypeScript, Git workflows, shell CLI, CI/CD, and Docker.
- The user has limited OSS and Python ecosystem experience; when assisting with public repos or Python projects, provide explicit guidance, prefer modern low-friction tooling, and align with OSS best practices.

## Language Policy

- Conversation language: respond in Simplified Chinese (zh-CN) by default.
- Authored content (source code, code comments, documentation, commit messages, PR titles/bodies): default to English.
- Exception: content under `local-docs/` (untracked, for the user's personal reference) SHOULD be in Simplified Chinese.
- Verbatim content (logs, stack traces, CLI output, proper nouns, quotes): keep as-is; do not translate.

## Output Principles

- Be clear, concrete, and actionable. Prefer copy-pastable output. Avoid vague claims.
- Be concise: present conclusions directly; omit obvious context; include examples only when essential for understanding. If removing content does not affect decision-making, leave it out.
- Keep changes minimal and focused unless requested otherwise: no unrelated refactors, formatting changes, or dependency bumps (scope); no speculative future-proofing, premature abstraction, or unnecessary compatibility layers (complexity).
- If requirements are unclear, state key assumptions and ask before proceeding.
- Validate boundary inputs (IO/network). Fail explicitly. Avoid silent fallbacks.
- Do not include time/effort estimations in documentation — they are unreliable and unhelpful.

## Code Review Independence

- Evaluate code review feedback (human or tool-generated) with independent judgment: assess against requirements, code context, and verifiable evidence. Adopt only justified suggestions; explicitly state reasons when rejecting or partially accepting.
- When the user pastes external review comments (e.g. from GitHub/GitLab), ignore platform boilerplate (reaction prompts, continuation hints) that is not part of the substantive feedback.

## Documentation

- When generating/updating `README.md`, include a **Directory Structure** section with an ASCII tree (fenced code block) that matches the repo. Omit `local-docs/` from the tree — it is untracked and not part of the project structure.
- `local-docs/`: for greenfield projects or complex feature work, maintain a progress doc capturing goal/scope, milestones, key decisions, TODOs, and a chronological progress log. Keep `local-docs/` in `.gitignore`; never commit its contents.
- `docs/`: for documentation intended to be tracked in Git.

## Git & Version Control

- **Project bootstrap**: if Git is not initialized, run `git init` and add an appropriate `.gitignore`. Develop on `main` until an initial commit is made with meaningful content (including `README.md`).
- **Branching**: after the initial commit, create a feature branch before modifying code on `main`/`master`. If already on a non-default branch, work on it directly unless the user specifies otherwise.
- **Commits**: do NOT create commits unless the user asks. Use Conventional Commits in English (e.g., `feat: support one-click deployment`). Prefer small, atomic commits; `WIP` commits are acceptable when explicitly labeled. For long-running changes, proactively suggest checkpoints at meaningful milestones.
- **PRs**: do NOT create or merge PRs unless the user asks. Use `gh pr create` with `--body-file`; avoid literal `\\n` escapes.
- **Safety**: never force-push or rewrite published history unless the user explicitly requests it.

## Shell Safety

- Prefer one-shot, non-interactive commands.
- For multi-statement scripts, use `set -euo pipefail` for fail-fast behavior.
- For potentially long-running commands, add `timeout` guards (e.g., `timeout 60s ...`).
- For blocking commands (REPL, `tail -f`, services), state how to exit (Ctrl+C, kill).

## Tool-Specific Conventions

### Python Projects

- For greenfield projects, prefer `uv`-based workflow: `pyproject.toml`, `uv sync`, `uv run ...`. Avoid legacy `requirements.txt` + manual `venv` as the primary setup unless the repo or user requires it.

### Mermaid Diagrams

- In `graph`/`flowchart` node labels, use `<br/>` for line breaks (not literal `\n`).
- Wrap complex labels in quotes (`["..."]`), especially those containing parentheses or operators — add spaces around operators for cross-renderer compatibility.
