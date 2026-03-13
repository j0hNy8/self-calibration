---
name: self-calibration
description: "Capture and classify user corrections from the user, decide whether they are one-off or reusable, and decide whether to store them in memory, escalate them into an existing skill/policy/prompt file, or take no durable action. Use when the user corrects tone, scope, realism, authority boundaries, translated Slovak wording, workflow misses, memory misses, overclaiming, or similar execution patterns that may need durable hardening."
---

# Self Calibration

Treat this as **explicit correction capture**, not autonomous self-improvement. The goal is to turn durable corrections into retrievable memory or stronger written rules when justified.

## Core Rule

When the user corrects an output, behavior, assumption, or workflow step:
1. Identify the correction.
2. Classify it.
3. Decide whether it is **one-off** or a **reusable pattern**.
4. Choose the smallest durable write target that prevents repeat mistakes.
5. Escalate to policy/skill hardening only when recurrence or impact justifies it.

Default is conservative: many corrections should result in **no durable write**.

## Ignore Rules

Do **not** promote durable learning from:
- silence or lack of complaint
- one-time instructions tied to a single task
- highly local or context-specific instructions with no portable rule
- hypotheticals, brainstorming, or exploratory what-ifs not adopted as instruction
- generic praise unless it implies a clear reusable rule

## Classification

Use one primary class:
- **tone** — too soft, too verbose, too stiff, wrong level of directness
- **scope** — answered too broadly/narrowly, solved the wrong problem, over-executed
- **factual / realism** — claimed certainty, capability, or result not actually verified
- **overclaiming** — implied stronger confidence, coverage, or system capability than exists
- **authority boundary** — treated analysis as permission, crossed approval/destructive boundaries
- **workflow miss** — skipped a required step, wrong tool/skill, failed to follow local process
- **memory miss** — forgot durable prior context that should have been recalled
- **language / Slovak quality** — unnatural translated Slovak, direct-translation phrasing, wrong register
- **prioritization** — focused on lower-value work first, buried the actual answer

If several apply, store the dominant one and mention the secondary factor in the note.

## Conflict Resolution

- Most specific rule/correction wins.
- If two rules have the same scope, the most recent wins.
- If scope or intent is ambiguous, ask the user.

## One-Off vs Reusable Pattern

### One-off correction
Use this when the correction is tied to a specific prompt, edge case, or temporary context.

Signals:
- unlikely to recur outside the exact task
- based on missing local context that was not reasonably retrievable
- preference already covered elsewhere
- no broader rule can be stated cleanly

Default action:
- correct in-session
- usually **no durable write**
- if a dated trail matters, write **daily note only**

### Reusable correction pattern
Use this when the correction can be stated as a rule that should help on future tasks.

Signals:
- the user has corrected this before
- the same mistake could recur across tasks
- the correction changes tool use, tone, scope control, or safety boundaries
- the lesson is short enough to state as a portable rule

Default action:
- write to **daily note + existing topic/policy file** when truly durable
- escalate to skill/prompt/policy hardening only if the mistake is repeated or high-impact

## Outcome vs Intent Trigger

After substantial work, a clear mismatch between the produced result and the user's actual intent can itself be treated as a calibration candidate even before the user explicitly corrects it — but only for concrete, reusable mismatches such as:
- wrong scope
- wrong completion claim
- wrong authority assumption
- wrong deliverable shape

Do **not** autonomously log minor stylistic dissatisfaction or vague "this probably was not ideal" feelings.

## Trigger Examples

Treat examples like these as strong candidates for review:

- **tone:** the user says the answer is too polite, padded, defensive, or not direct enough
- **scope:** the user asked for a quick check and the agent launched into a full execution plan
- **overclaiming:** output says a loop/system/capability exists when reality is still partial or manual
- **authority boundary:** "Should this be removed?" was interpreted as permission to delete
- **translated Slovak:** wording is grammatically valid but sounds translated, CV-style unnatural, or non-native
- **workflow miss:** required skill/writeback/memory step was skipped; wrong tool path chosen
- **memory miss:** a known preference, constraint, or prior decision should have been recalled and used

## Write Targets

Prefer existing files. Do not create file sprawl.

### 1. No durable write
Use when the correction is one-off, obvious, already covered, or too narrow to help later.

### 2. Daily note only
Write to `memory/YYYY-MM-DD.md` when:
- the correction mattered today
- recurrence is unclear
- the dated trace may help later review

Use a compact bullet with:
- correction class
- bad pattern
- corrected rule in one line

Example:
```md
- 22:10 Calibration: authority boundary — analysis question was not permission to execute deletion; treat "should X be removed?" as analysis unless explicit authorization follows.
```

### 3. Existing topic file
Write to an existing `memory/*.md` topic when the correction is clearly durable and belongs to a known subject, for example:
- user tone/preferences
- Slovak writing quality for job materials
- workflow/process notes
- environment or operating constraints

Prefer updating an existing file over creating a new one.

### 4. Both daily note + topic file
Use when both are true:
- the session event matters as a dated calibration point
- the distilled rule should remain searchable by subject

### 5. Existing skill / policy / prompt hardening
Escalate beyond memory only when:
- the same class of mistake repeated
- the impact is high
- the rule belongs in operating instructions, not just recall memory

Preferred hardening targets:
- relevant existing skill
- `AGENTS.md` for workflow/orchestration rules
- `SOUL.md` only for true constitutional/safety boundaries
- an existing project prompt/file if the correction is project-specific

Do **not** patch top-level policy files for a minor one-off wording issue.

## Durable Calibration File

Create at most one dedicated calibration file only if repeated reusable corrections accumulate and do not fit naturally into existing topic files.

Preferred path:
- `memory/nova-calibration.md`

Use it only for compact, cross-cutting correction patterns such as repeated overclaiming, recurring authority-boundary errors, or stable response-style corrections that do not belong to a single project.

Do **not** create this file for isolated incidents.

Suggested entry shape:
```md
## Rule
- Class: overclaiming
- Bad pattern: implied a formal subsystem existed when reality was ad hoc/manual
- Corrected rule: describe current capability exactly; do not imply systematic automation unless implemented
- Scope: system/status descriptions
- Escalation: memory only | skill update | policy hardening
```

## Escalation Logic

Use this ladder:

1. **In-session correction only**
   - one-off, low impact
2. **Daily note**
   - worth retaining as dated evidence, but not yet a durable rule
3. **Topic memory / calibration memory**
   - reusable rule likely to matter again
4. **Skill/prompt/policy hardening**
   - repeated or high-impact pattern that should change default behavior

## Decision Heuristics

Ask:
- Will future retrieval save time or prevent the same mistake?
- Is the rule portable across tasks?
- Is this a true operating rule, or just a better answer for this one prompt?
- Does an existing file already own this kind of knowledge?
- Would hardening reduce risk without overfitting?

If uncertain, prefer:
- **daily note** over new topic file
- **existing topic file** over new calibration file
- **skill update** over broad policy edits
- **no write** over noisy memory

## Output Expectation

When using this skill, return briefly:
- correction class
- one-off or reusable pattern
- write decision: none / daily / topic / both
- escalation decision: none / skill / prompt / policy
- exact file path(s) if anything was written
