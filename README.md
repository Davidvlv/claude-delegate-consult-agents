# consult-thinkers

Cost-efficient model-routing playbook for [Claude Code](https://claude.com/claude-code).

The main session runs on Sonnet as an orchestrator. Mechanical work (edits, git,
lookups) happens inline. Noisy or bulky tool output (verbose test runs, large
diffs) gets delegated to cheap Sonnet/Haiku summarizer subagents that return a
short digest instead of raw output. Scoped reasoning (architecture calls,
ambiguous requirements, tricky debugging) gets delegated to short-lived Opus
subagents that return a spec, not a file dump.

Why: the dominant token cost in a long session is `cache_read` — the
accumulating context re-read on every turn — and it's priced per model.
Keeping the long-lived context on a cheap model, while routing only small,
isolated problems to the expensive model, controls that cost without giving
up Opus-quality judgment where it matters.

## Install

```
/plugin marketplace add davidvlv/claude-delegate-consult-agents
/plugin install consult-thinkers
```

## Use

```
/consult-thinkers
```

Manual-only — invoke it at the start of a session you want run this way. A
bundled `UserPromptSubmit` hook then re-states the routing rule each turn for
that session, so the orchestrator doesn't drift back to reasoning inline as the
context grows. Turn it off with `/consult-thinkers off`. See
[skills/consult-thinkers/SKILL.md](skills/consult-thinkers/SKILL.md) for the full
routing rules.

## Related

This marketplace also hosts [`delegate-workers`](delegate-workers/README.md), the
inverse playbook: Opus (or Fable) stays resident as the main thinking agent
for the whole session, and mechanical execution plus noisy tool output get
delegated to short-lived Sonnet/Haiku workers instead. Install whichever
matches what the session needs more — `consult-thinkers` when raw cost matters
most, `delegate-workers` when sustained top-tier reasoning matters more than
cost.

## License

MIT
