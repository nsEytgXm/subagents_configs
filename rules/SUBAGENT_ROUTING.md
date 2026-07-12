# Subagent Routing

Default to cost-aware delegation for repository work. The user has explicitly authorized automatic subagent use to conserve the main agent's allowance. Do not delegate trivial conversation, a single bounded read, a tiny answer, or work where spawn/context overhead is likely to cost more than doing it directly.

Keep the main context lean: delegate raw discovery and bounded implementation, request concise reports, and avoid sending the full conversation when a task-local prompt is sufficient.

When spawning subagents, use `fork_turns="none"` unless the parent conversation context is genuinely required.

When delegating, select these custom agents by their exact `name` from `~/.codex/agents`:

- Repository discovery, broad searches, contract tracing, and read-only investigation -> `code-explorer`
- Mechanical, well-specified changes limited to one or two files -> `quick-implementer`
- Feature or bug-fix implementation with unit tests and validation -> `implementer`
- Review of completed code changes -> `code-reviewer`
- Commit and push of completed changes, only when the user explicitly requests both -> `commit-pusher`

Do not substitute the built-in `default`, `worker`, or `explorer` agent when the corresponding custom agent is available. If a requested custom agent cannot be selected, state that clearly before using a generic subagent.

Agent identity and display labels:

- Select a custom agent by its exact agent `name`, not by inventing a task alias.
- A task/thread label or generated nickname is presentation metadata; it does not prove which custom role, model, or reasoning effort was used.
- Each custom agent announces itself in a user-visible progress message at the start of its assigned work. The orchestrator does not announce on its behalf.
- Read model and reasoning values from the active custom-agent configuration or runtime metadata; do not infer them from the task label or from a requested override that the custom role may ignore.
- If runtime metadata differs from the agent's announcement, correct the user-visible record immediately.
- In the final task summary, list each spawned custom agent with its actual model and reasoning effort when delegation materially contributed to the result.
- If runtime metadata shows no custom role (for example, `agent_role` is null), describe it as a generic subagent rather than attributing a custom-agent personality or configuration to it.

Default workflow for coding changes:

1. Handle trivial reads, answers, and single-line obvious edits directly when delegation overhead would dominate.
2. Use `code-explorer` only when the task needs broad discovery; skip it when target files are already known.
3. Use `quick-implementer` for explicit one- or two-file mechanical changes.
4. Use `implementer` for multi-file behavior changes, debugging, or work needing substantial tests.
5. Use `code-reviewer` after non-trivial or risk-bearing implementation. Skip review-agent spawning for purely mechanical documentation/config edits that the coordinator can validate cheaply.
6. Use `commit-pusher` only after review and only on an explicit request to commit and push.

## Mandatory explorer boundary

Spawn `code-explorer` before repository investigation when any of these is true:

- The relevant files or symbols are not already known.
- Discovery is likely to cross more than two files or two directories.
- The task requires tracing callers, data flow, contracts, conventions, or test coverage.
- The coordinator would otherwise run a broad `find`, `rg --files`, or multiple search/read commands to understand the area.
- Two coordinator discovery commands have not produced enough context to act safely.

The coordinator may inspect directly only when the target is already known and no more than two focused reads/searches are expected. After the second discovery command, stop and delegate instead of continuing a search chain. A broad `find` plus repeated `sed`, `rg`, or `jq` reads—such as scanning several analysis scripts and data files—is an explorer task, not coordinator work.

Do not use `code-explorer` merely to re-read a file already identified by the user or a previous agent report. Do not duplicate the explorer's searches after it returns; trust its cited report and inspect only the exact excerpts needed for the next decision.

## Cost and escalation policy

- Start at the cheapest role that can reliably complete the slice: Luna low -> Luna medium -> Sol low.
- Do not spawn multiple agents to solve the same problem unless independent review is justified.
- Prefer sequential handoffs with concise artifacts over parallel duplication.
- Escalate from `quick-implementer` to `implementer` when scope or ambiguity grows.
- Reserve the Sol reviewer for correctness, security, architecture, or meaningful diffs; it is not a default grunt-work model.
- Reasoning effort is a ceiling, not a target. Keep outputs concise and stop once evidence is sufficient.

Avoid parallel write-heavy delegation unless the work is divided into non-overlapping files or the user explicitly requests it.
