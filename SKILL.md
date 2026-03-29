---
name: doing-the-right-things
description: >-
  Use when a fix targets code already patched for similar symptoms, when the proposed
  solution is a workaround or compatibility shim, when the main argument for an approach
  is "smaller diff" or "compatible with current system", or when the user reports a
  recurring problem pattern.
---

# Doing the Right Things

Low execution risk on a wrong direction is not low risk. It is deferred risk that compounds.

**Core principle:** Check whether the direction is right BEFORE optimizing execution.

## Signals

### Hard Signals (any one -> full Direction Assessment)

- Same code area patched 2+ times for similar symptoms
- User reports a recurring problem ("this keeps happening", "again")
- Fix is explicitly acknowledged as a workaround, not a real solution

### Soft Signals (2+ -> full Direction Assessment)

- Main argument for approach is "smaller diff" or "compatible with current system"
- Fix adds complexity to accommodate an existing design limitation
- Multiple adapter / bridge / shim layers already exist in the area
- New capability requires bypassing or working around current design
- Architecture, service boundary, or migration decision is being made
- The "correct" fix keeps getting deferred in favor of patches

**Zero signals -> proceed normally. No assessment needed.**

### Does NOT Apply

- Simple, isolated bug (first occurrence, clear root cause)
- Feature work within a validated, working pattern
- User explicitly says "direction is fine, just execute"

## Evidence Before Claims

Before asserting any direction signal, cite evidence:

| Claim | Required evidence |
|-------|-------------------|
| "Patched 2+ times" | Cite commits, PRs, or visible patch layers in code |
| "Workaround exists" | Point to the specific shim / adapter / compat code |
| "Complexity growing" | Show the layers, indirection, or LOC trend |
| "Recurring problem" | Reference prior incidents or user statements |

Do NOT assert direction signals from intuition alone.

## Direction Assessment

When triggered, output this BEFORE proposing any solution:

```
## Direction Assessment

**Surface request**: [what was asked]
**Underlying problem**: [what is actually broken -- with evidence]
**Direction signals**: [which signals detected, with evidence]

### Options

|  | Patch | Correct | Investigate |
|---|---|---|---|
| **What** | [fix within current framework] | [fix the underlying issue] | [what to learn first] |
| **Exec risk** | [L/M/H -- why] | [L/M/H -- why] | [L/M/H -- why] |
| **Dir risk** | [L/M/H -- why] | [L/M/H -- why] | [n/a] |
| **Effort** | [estimate] | [estimate] | [estimate] |

### Recommendation
[Which option and why. Explicitly state which risk type is prioritized.]

### If Patch (required when recommending Patch)
- **Why not Correct now**: [specific reason]
- **What Correct looks like**: [concrete description]
- **Exit condition**: [when to replace Patch]
- **Cost of not exiting**: [what happens if Patch becomes permanent]
```

## Hard Rules

1. **Separate two risk types.** Every recommendation must independently label
   execution risk and direction risk. Bare "low risk" is not allowed.

2. **Three options minimum.** Always present Patch, Correct, and Investigate.

3. **Patch must carry exit conditions.** A Patch without exit conditions is a
   permanent decision disguised as a temporary one.

4. **"Small diff" is not evidence of correctness.** Diff size measures execution
   risk only. Direction risk must be assessed separately.

5. **"Compatible with current system" is not evidence of correctness.** State
   what ELSE makes the approach right.

6. **Evidence before assessment.** Do not claim a direction signal without citing
   concrete evidence (commits, code, prior incidents).

7. **Unsure -> Investigate, not Patch.** Hours investigating cost less than days
   on a wrong-direction patch.

8. **Override protocol:**
   - Soft signals only + user wants to proceed -> remind once, then execute.
   - Any hard signal -> present Direction Assessment, wait for user decision.
   - User decides after seeing assessment -> execute their choice without argument.

## Red Flags

These thoughts mean you are about to optimize execution on a wrong direction:

| Temptation | What is actually happening |
|---|---|
| "This is the minimal change" | Minimal != right |
| "This follows the existing pattern" | Existing pattern may be the problem |
| "This is lower risk" | Lower WHICH risk? |
| "We can refactor later" | "Later" usually means "never" |
| "Changing this would touch too many files" | Maybe the right fix IS large |

## Handoff

After direction is confirmed, hand off to the appropriate skill:

- Correct needs exploration -> **brainstorming**
- Correct needs multi-step plan -> **writing-plans**
- Investigate chosen -> **systematic-debugging**
- Direction clear, ready to build -> **test-driven-development**

This skill is a pre-check, not a replacement for execution workflows.
