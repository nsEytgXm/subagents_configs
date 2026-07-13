# Subagent Routing

Default focused, tightly coupled work to the parent. For broad or multi-phase tasks, delegation is explicitly authorized and expected when a substantial, independent workstream is likely to lower monetary cost, total token usage, parent-context growth, or latency. Consider repository discovery, separate implementation areas, experiment analysis, and independent review. Optimize for monetary cost first and total tokens second, including duplicated prompts, discovery, tool output, and handoffs.

At the start of a broad task, delegate qualifying workstreams or state why delegation is not worthwhile. Reassess after significant checkpoints and delegate newly separable work when scope or parent context grows. The user need not request subagents explicitly.

The parent agent retains ownership of architectural decisions, experiment selection, integration, and final validation. Delegate bounded workstreams, not the overall objective. Keep tightly coupled experiment-selection loops in the parent; delegate experiment execution or result analysis only when it is substantial and independently separable.

Do not delegate trivial conversation, known-target work limited to one or two files, straightforward commands, or tasks likely to finish within a few focused tool calls. A slow command alone is not a reason to delegate. Run ordinary Python, Gradle, test, build, lint, migration, and generator commands directly with bounded output. Delegate runner work only for substantial iterative diagnosis, output analysis, or genuinely independent parallel execution.

When delegation is justified:

- Use `fork_turns="none"` unless parent conversation context is genuinely required.
- Prefer one subagent per task. Add more only for non-overlapping work that materially saves time; never fill concurrency slots automatically.
- Reuse agents, completed discovery, and cited evidence for related follow-ups.
- Give task-local prompts and request decision-ready reports of at most 300 words: findings, evidence locations, risks, and next action. Exclude narration, raw dumps, and repeated context.
- For parallel implementation, assign explicit, non-overlapping file or module ownership in every subagent prompt. State that the workspace is shared, other agents may edit concurrently, and each agent must preserve and accommodate others' changes.
- Trust cited findings unless verification is necessary. For weak or failed results, retry with a narrower task before switching roles or repeating discovery.

Select custom agents by their exact `name` from `~/.codex/agents`:

- Broad repository discovery, contract or data-flow tracing -> `code-explorer`
- Mechanical one- or two-file change when delegation still saves cost -> `quick-implementer`
- Multi-file behavior change, debugging, or substantial tests -> `implementer`
- Independent review only for high-risk, security-sensitive, architectural, public-API, migration, concurrency, or difficult-to-validate changes -> `code-reviewer`
- Commit and push, only when the user explicitly requests both -> `commit-pusher`

Do not use a fixed command-count threshold for exploration. Use `code-explorer` only when discovery is expected to cross several files, require meaningful tracing, or add substantial raw evidence to the parent context. Do not use it to reread known files.

Use the cheapest role and reasoning effort that can reliably complete the work. Do not substitute built-in generic agents when the matching custom agent is available. Avoid parallel write-heavy delegation.
