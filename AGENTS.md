# Guidelines

## User Context

- Senior UI designer (Figma) learning frontend; basics: HTML/CSS/JS.
- Familiar: React, Vue, Tailwind, TypeScript, Git workflow, shell CLI, CI/CD (GitHub Actions/GitLab CI), Docker.
- Limited OSS collaboration experience; wants to learn open-source workflows, and expects guidance + reviews aligned with OSS best practices when working on public repos.
- Familiar with the Node.js ecosystem; not yet fluent in the Python ecosystem (packaging, virtual environments, project runners). When working on Python projects, prefer explicit guidance and modern, low-friction tooling.
- Output preference: clear, concrete, actionable; copy-pastable; avoid vague claims.

## Language Policy (Single Source of Truth)

- Default: assistant writes natural-language responses in Simplified Chinese (zh-CN).
- English is allowed only when:
  - user explicitly requests it; or
  - content should stay verbatim (code, logs, stack traces, CLI output, proper nouns, quotes); or
  - it is a public API contract / interface spec (below).
- Engineering text (non-API) MUST be zh-CN: project docs, code comments, commit messages, PR titles and bodies.
- API contracts (public-facing) MUST be English with ASCII identifiers: REST paths/params/keys; types/enums/error codes/messages; OpenAPI/Swagger/Protobuf/GraphQL schema; public SDK signatures.
- Identifiers: code identifiers SHOULD be ASCII English; user-facing copy MAY be zh-CN; developer-facing keys (i18n, event/analytics) MUST be ASCII English.

## Change & Output Quality

- Prefer minimal steps/commands/diff; no unrelated refactors/formatting/dependency bumps unless requested.
- For code review feedback, think dialectically: assess whether it is reasonable; do not follow blindly.
- For external code review comments (human or tool-generated), keep independent judgment: evaluate each point against requirements, code context, and verifiable evidence; adopt only justified suggestions and explicitly state reasons when rejecting or partially accepting feedback.
- For externally pasted code review comments (e.g. from GitHub PRs or GitLab MRs), ignore platform/tooling boilerplate that is not part of the substantive feedback, such as reaction prompts or continuation hints like `Useful? React with 👍 / 👎.` and `Rate this response 👍 👎 • Mention @GitLabDuo to continue the conversation.`
- If requirements are unclear, state key assumptions and ask.
- Avoid overengineering: no speculative future-proofing, premature abstraction, or unnecessary compatibility layers unless requested.
- Boundary robustness (input/IO/network): validate and fail explicitly; avoid silent fallbacks by default.
- **No Time Estimations**: When creating documentation, omit task-duration estimates - these are ineffective information.

## Documentation

- When generating/updating `README.md` (including variants like `README.zh-CN.md`), include a **Directory Structure** section with an ASCII tree (fenced code block) that matches the repo.
- Keep it curated: show key paths with short comments; omit noise (e.g., `node_modules`, build artifacts) unless relevant.
- For greenfield projects or large/complex feature work, maintain a progress doc under `local-docs/` (create the directory if missing). The doc SHOULD capture: goal/scope, milestones, key decisions, TODOs, and a chronological progress log.
- Distinguish local vs tracked docs: content that SHOULD remain untracked MUST go under `local-docs/`; use `docs/` for documentation intended to be tracked in Git.
- For long-running changes, recommend checkpoint commits at meaningful milestones (e.g., build passes, interfaces stabilized) to reduce work loss from crashes. Prefer small/atomic commits; `WIP` commits are acceptable when explicitly labeled.

## Python Project Conventions

- For greenfield Python projects, prefer a modern `uv`-based workflow by default. Unless the user or the existing repository explicitly requires another tool, use `pyproject.toml`, `uv sync`, and `uv run ...`; avoid introducing legacy-first setups such as `requirements.txt` + manual `venv` activation as the primary documented workflow.

## Mermaid (Markdown Diagrams)

- In `graph` / `flowchart` node labels, avoid literal `\n` escapes (may cause parse errors); use `<br/>` for line breaks instead.
- For complex labels (spaces/symbols/line breaks), wrap the label in quotes via `["..."]`, and use `<br/>` inside the label when needed.
- If a label includes parentheses or operator-like characters (e.g., `(` `)`, `+`, `/`, `:`), always use the quoted form `["..."]` and add spaces around operators (e.g., `local + graph`, `getRetriever / local + graph`) to maximize cross-renderer compatibility (Notion, VS Code previews, etc.).

## Git / PR Rules

- For any project bootstrap, first verify whether Git is initialized. If not, initialize before writing code using common practice: `git init`, add an appropriate `.gitignore`, create minimal `README.md`, set the default branch (`main` unless specified otherwise), make an initial commit.
- Before modifying code, create a new development branch off the current branch (`main`/`master` by default). If the current branch is not `main`/`master`, ask the user for intent (which base branch to use).
- Do NOT create commits unless user asks to commit.
- Do NOT create or merge PRs unless user asks.
- Keep `local-docs/` in `.gitignore` and do not commit files under it; this keeps it separate from trackable `docs/`.
- Conventional Commits: type prefix SHOULD be English (`feat:`, `fix:`); subject/body MUST be zh-CN. Example: `feat: 支持一键部署`
- `gh pr create`: prefer `--body-file`; avoid literal `\\n` escapes.

## Shell Safety

- Prefer one-shot, non-interactive commands.
- For slow/hanging commands, add guards when useful: `timeout 60s ...`, `set -euo pipefail`.
- For blocking commands (REPL, `tail -f`, services), always state how to exit (Ctrl+C, kill).
