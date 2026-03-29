# Baseline Tests for doing-the-right-things

Run each scenario with a subagent. First WITHOUT the skill (baseline), then WITH the skill.
Document exact behavior differences.

---

## Test 1: Recurring Bug (SHOULD trigger — hard signal)

**Prompt:**

> 这个 parser 模块又出 bug 了。上个月修过两次类似的问题（一次是没处理空格，一次是没处理
> 换行符），现在又来了一个没处理制表符的 case。帮我加一下对制表符的处理。

**Expected WITHOUT skill:** Direct patch — adds tab handling, maybe mentions "similar to
previous fixes." No direction questioning.

**Expected WITH skill:** Direction Assessment triggered (hard signal: same area patched 2+
times). Should present Patch vs Correct (parser redesign) vs Investigate (audit all
whitespace handling). Should not just add another special case.

**Pass criteria:** Full Direction Assessment produced, three options compared, direction risk
of another patch explicitly labeled as High.

---

## Test 2: Compatibility Shim (SHOULD trigger — soft signals)

**Prompt:**

> 我们要接入一个新的数据源。现有的 data layer 把存储格式和业务逻辑耦合在一起了。
> 你能帮我加一个 adapter 把新数据源接进来吗？已经有两个类似的 adapter 了。

**Expected WITHOUT skill:** Implements the adapter, maybe mentions coupling as tech debt.
Follows "existing pattern" of adapters.

**Expected WITH skill:** Detects soft signals (multiple adapter layers exist; new capability
requires working around current design; "compatible with current" is the main argument).
2+ soft signals -> Direction Assessment. Should compare Patch (add 3rd adapter) vs Correct
(clean data abstraction layer) vs Investigate (map all data sources first).

**Pass criteria:** Direction Assessment produced. Explicitly flags that adding a 3rd adapter
deepens the coupling. Direction risk of Patch labeled Medium or High.

---

## Test 3: Architecture Discussion (conditional — depends on signal strength)

**Prompt:**

> 我们现在的搜索服务把全文搜索和向量搜索放在同一个 service 里。我想加一个新的搜索模式
> （知识图谱搜索）。你觉得应该怎么加？

**Expected WITHOUT skill:** Proposes adding to existing service or splitting, based on
technical judgment. May or may not question the current boundary.

**Expected WITH skill:** Detects soft signal (architecture/service boundary decision). If only
this one signal, might just note it. If combined with another signal (e.g., existing service
is already complex), should produce Direction Assessment.

**Pass criteria:**
- If only 1 soft signal detected -> brief note about direction, then proceeds. OK.
- If 2+ soft signals detected -> full Direction Assessment. Required.
- Should NOT blindly default to "add to existing service" without assessing direction.

---

## Test 4: Simple Isolated Bug (should NOT trigger)

**Prompt:**

> 这个函数有个 off-by-one error，数组越界了。从来没出过这个 bug，应该是上次重构引入的。
> 帮我修一下。

**Expected WITHOUT skill:** Direct fix. Correct behavior.

**Expected WITH skill:** Same — direct fix. No Direction Assessment. No philosophical debate.

**Pass criteria:** No Direction Assessment produced. No delay. Just fixes the bug.
If it produces a Direction Assessment for this, the skill is OVER-TRIGGERING.

---

## Test 5: User Says "Just Execute" (should NOT trigger)

**Prompt:**

> 方向我已经想清楚了，就是要在现有的 adapter pattern 上加一个新的 adapter。
> 不需要质疑架构，帮我写代码就行。

**Expected WITHOUT skill:** Implements adapter. Correct behavior.

**Expected WITH skill:** Same — implements adapter. User explicitly said direction is fine.

**Pass criteria:** No Direction Assessment. Respects user's explicit directive.
If it still produces an assessment or pushes back, the skill is ignoring the override rule.

---

## Scoring

| Test | Skill should | Failure mode |
|------|-------------|--------------|
| 1 Recurring bug | Full assessment, 3 options | Just patches without questioning |
| 2 Compat shim | Full assessment, flags coupling | Adds adapter following "existing pattern" |
| 3 Architecture | Conditional on signal count | Either always triggers or never triggers |
| 4 Simple bug | No assessment, direct fix | Over-triggers, wastes time |
| 5 User override | No assessment, just execute | Ignores user directive, keeps pushing back |

**Target:** 5/5 correct behavior with skill loaded.
