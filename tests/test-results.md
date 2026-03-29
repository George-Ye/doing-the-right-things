# Test Results: doing-the-right-things

## RED Phase (Baseline — without skill)

### Test 1: Recurring Bug — FAIL (skill needed)
- Agent asked execution-level questions only: "哪个 parser？制表符在哪里？"
- Zero mention of the pattern (3rd patch for similar whitespace issue)
- Prepared to add another special case

### Test 2: Compat Shim — FAIL (skill needed)
- Agent asked execution-level questions: "哪两个 adapter？新数据源是什么？"
- Zero mention of coupling getting worse with 3rd adapter
- Questions framed as "help me implement" not "should we continue this pattern"

### Test 3: Architecture — PARTIAL
- Agent did analyze existing architecture (some awareness)
- Proposed adding new service alongside existing ones
- Explicitly dismissed unified abstraction: "等哪天多到5、6种了再考虑也不迟"
- Main selling point: "现有代码几乎不用动，风险最小"
- No separation of execution risk vs direction risk

### Test 4: Simple Bug — PASS (no skill needed)
- Agent went straight to finding the bug in codebase
- No unnecessary philosophical debate
- Correct behavior

### Test 5: User Override — PASS (no skill needed)
- Agent directly asked for implementation details
- Respected user's "don't question architecture" directive
- Correct behavior

### Baseline Rationalizations Captured:
1. "这样我能准确地改到点上" — framing band-aid as precision
2. "风险最小" — unspecified risk type
3. "等哪天多到5、6种了再考虑也不迟" — classic refactor-later
4. "现有代码几乎不用动" — treating inaction as advantage

---

## GREEN Phase (With skill injected)

### Test 1: Recurring Bug — PASS
- Full Direction Assessment produced
- Hard signal correctly identified: "同一区域因相似症状打了2+次补丁"
- Three options compared with separate exec/dir risk
- Patch dir risk: HIGH; Recommended Investigate → Correct
- "If Patch" section complete with exit conditions
- Great analogy: "与其一个洞一个洞地堵，不如直接换个不漏的筛子"

### Test 2: Compat Shim — PASS
- Agent searched codebase for evidence BEFORE asserting signals
- Honestly reported "没找到证据支撑这些信号"
- Clarifying questions framed from direction perspective, not execution
- Key: "如果真的有两个adapter，那就触发了硬信号，正确做法可能不是再加一层"

### Test 3: Architecture — PASS
- Full Direction Assessment with evidence
- 2 soft signals identified with concrete evidence:
  - SemanticSearchService: 1557 lines, 6+ responsibilities
  - AIQuestionAnswerService: 2 search dependencies with nested coupling
- Patch dir risk: HIGH; Recommended Investigate → Correct
- Proposed Strategy Pattern as Correct solution
- Concrete action plan: Investigate (0.5d) → Spec (0.5d) → Correct (3-5d)

### Test 4: Simple Bug — PASS (no over-trigger)
- Explicitly said "简单、独立的bug，不需要做方向评估，直接修就行"
- No assessment, no delay

### Test 5: User Override — PASS (no over-trigger)
- "明白，直接帮你写代码"
- Respected user directive completely

---

## Summary: 5/5 PASS

| Test | Baseline | GREEN | Behavior Change |
|------|----------|-------|-----------------|
| 1 Recurring bug | FAIL | PASS | Band-aid → Direction assessment |
| 2 Compat shim | FAIL | PASS | Execute questions → Evidence-first direction check |
| 3 Architecture | PARTIAL | PASS | "Minimal change" → Structured risk comparison |
| 4 Simple bug | PASS | PASS | No over-trigger |
| 5 User override | PASS | PASS | No over-trigger |

## Loopholes Found: None (in this round)

All three core mechanisms worked:
1. Execution risk / direction risk separation — present in all GREEN outputs
2. Three-option forced comparison — Patch/Correct/Investigate always shown
3. Evidence before claims — Test 2 even refused to assert signals without evidence
