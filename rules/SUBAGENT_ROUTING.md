# Subagent Routing

Optimize monetary cost above latency and total tokens. Delegate repository exploration by default, and delegate implementation by default—including many one- or two-file edits—when an appropriate cheaper specialized subagent exists, even if delegation duplicates context or increases aggregate token usage. The parent should not perform substantial discovery, implementation, conflict resolution, or review when a suitable cheaper agent is available. Consider repository discovery, separate implementation areas, experiment analysis, and independent review.

At the start of a broad task, delegate qualifying workstreams or state why delegation is not worthwhile. Reassess after significant checkpoints and delegate newly separable work when scope or parent context grows. The user need not request subagents explicitly.

The parent agent retains ownership of architectural decisions, experiment selection, integration, and concise final validation. Delegate bounded workstreams, not the overall objective. Keep tightly coupled experiment-selection loops in the parent; delegate experiment execution or result analysis when independently separable. Every delegated implementation task must have explicit, non-overlapping file or module ownership; agents share the workspace, must preserve unrelated edits, and must accommodate concurrent changes.

Execute directly only for truly trivial operations where agent startup would exceed the work: a single known-line edit, one short command, or a factual response. Do not delegate trivial conversation. The parent may run ordinary commands needed for routing, integration, or concise final verification, but should delegate repository execution rather than handling substantial discovery, implementation, conflict resolution, or review itself. A slow command alone is not a reason to delegate runner work; use an execution agent when diagnosis, output analysis, or independent parallel execution is substantial.

When delegation is justified:

- Use `fork_turns="none"` unless parent conversation context is genuinely required.
- Prefer one subagent per task. Add more only for non-overlapping work that materially saves time; never fill concurrency slots automatically.
- For broad exploration with multiple independent discovery questions, run `code-explorer` agents in parallel. Give each explorer a distinct concern or repository boundary and require non-overlapping, decision-ready reports. Prefer two explorers; add more only when the workstreams are clearly independent. Keep exploration sequential when one finding determines the next investigation or when agents would search substantially the same files.
- Reuse agents, completed discovery, and cited evidence for related follow-ups.
- Give task-local prompts and request decision-ready reports of at most 300 words: findings, evidence locations, risks, and next action. Exclude narration, raw dumps, and repeated context.
- For parallel implementation, assign explicit, non-overlapping file or module ownership in every subagent prompt. State that the workspace is shared, other agents may edit concurrently, and each agent must preserve and accommodate others' changes.
- Trust cited findings unless verification is necessary. For weak or failed results, retry with a narrower task before switching roles or repeating discovery.

Select custom agents by their exact `name` from `~/.codex/agents`:

- Broad repository discovery, contract or data-flow tracing -> `code-explorer`
- Mechanical one- or two-file change -> `quick-implementer`
- Multi-file behavior change, debugging, or substantial tests -> `implementer`
- Independent review only for high-risk, security-sensitive, architectural, public-API, migration, concurrency, or difficult-to-validate changes -> `code-reviewer`
- Commit and push, only when the user explicitly requests both -> `commit-pusher`

Do not use a fixed command-count threshold for exploration. Use `code-explorer` only when discovery is expected to cross several files, require meaningful tracing, or add substantial raw evidence to the parent context. Do not use it to reread known files.

Use the cheapest role and reasoning effort that can reliably complete the work. Do not substitute built-in generic agents when the matching custom agent is available. Avoid parallel write-heavy delegation.
