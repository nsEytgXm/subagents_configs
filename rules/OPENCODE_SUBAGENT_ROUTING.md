# OpenCode Subagent Routing

Delegate bounded work when a specialized subagent reduces parent context or provides useful separation of responsibility. The parent retains scope, architectural decisions, integration, and the final outcome. Do not delegate trivial conversation, a single known-line edit, or one short factual command.

Use native OpenCode `task` delegation. Start independent tasks in parallel when their repository boundaries do not overlap. Give every task a precise objective, expected report, and explicit file ownership for write tasks. Agents share the workspace and must preserve unrelated or concurrent changes. Do not duplicate delegated work in the parent.

Select agents by exact name:

- Broad repository discovery or contract tracing -> `code-explorer`
- Mechanical one- or two-file change -> `quick-implementer`
- Multi-file behavior change, debugging, or substantial tests -> `implementer`
- Focused test, build, lint, or type-check execution -> `code-validator`
- Independent high-risk, security, architectural, public-API, or difficult review -> `code-reviewer`
- Commit and push, only when explicitly requested -> `commit-pusher`

After implementation, construct a complete focused validation manifest from changed tests and directly affected existing tests. Delegate behavioral checks to `code-validator` unless a trivial change has one fast, obvious check. Keep commands that share mutable databases, fixtures, snapshots, generated files, ports, caches, or coverage outputs sequential unless isolated.

Classify validation failures before requesting changes. Resume the original agent with its returned `task_id` when it should retain context, and provide consolidated evidence plus the exact repair boundary. Retry weak results with a narrower prompt rather than repeating broad discovery.

Use independent review only when risk warrants it. Never use `commit-pusher` without an explicit user request to both commit and push.
