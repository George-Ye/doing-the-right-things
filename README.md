# doing-the-right-things

A Claude Code skill that prevents high-quality execution on wrong directions.

> "如果方向错了，执行力越强，死得越快。" — 段永平

## What it does

Forces **direction validation before execution optimization**. When it detects signals that the current approach may be wrong (recurring patches, workaround-based fixes, "smaller diff" as main justification), it requires a structured Direction Assessment comparing:

- **Patch** (fix within current framework)
- **Correct** (fix the underlying issue)
- **Investigate** (learn more first)

Each option must separately label **execution risk** and **direction risk** — because low execution risk on a wrong direction is not low risk.

## Install

```bash
/plugin marketplace add George-Ye/doing-the-right-things
/plugin install doing-the-right-things@doing-the-right-things
```

## Key Rules

1. "Small diff" ≠ "low risk" — diff size only measures execution risk
2. "Compatible with current system" ≠ "correct" — compatibility with a wrong system is not a virtue
3. Always present three options: Patch, Correct, Investigate
4. Patch must carry exit conditions — no disguising permanent decisions as temporary
5. Evidence before claims — cite commits/code/incidents, not intuition

## Inspired by

- **段永平**: 做对的事情 → 把事情做对 → 知错就改
- **Peter Drucker**: "There is nothing so useless as doing with great efficiency something that should not be done at all."
- **Warren Buffett**: "When you find yourself in a hole, stop digging."

## TDD Tested

5/5 scenarios passed ([test results](tests/test-results.md)):

| Scenario | Should trigger? | Result |
|----------|----------------|--------|
| Recurring bug (3rd patch) | Yes | Correct — full Direction Assessment |
| Compatibility shim | Yes | Correct — evidence-first questioning |
| Architecture decision | Conditional | Correct — triggered on 2+ soft signals |
| Simple isolated bug | No | Correct — no over-trigger |
| User says "just execute" | No | Correct — respected override |
