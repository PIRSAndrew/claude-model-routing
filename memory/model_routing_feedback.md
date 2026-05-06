---
name: Model routing feedback log
description: Running log of cases where model tier was wrong or right — used to refine tier defaults over time.
type: feedback
---

# Model Routing Feedback Log

This file tracks every routing signal — positive confirmation, escalation (too shallow), and de-escalation (too slow/heavy). Over time it builds the empirical ground truth for what each task profile in your workflow actually needs.

**Place this file at:** `~/.claude/projects/<your-project>/memory/model_routing_feedback.md`

---

## Confirmed Tier Assignments

Update this table as you validate tier assignments through use. Pre-seeded rows are generic starting points — replace with task types specific to your workflow.

| Task Type | Correct Tier | Notes |
|-----------|-------------|-------|
| Report generation (established template) | Sonnet | Schema known, output format fixed |
| Data pulls / DB queries | Sonnet | No synthesis required |
| Code edits on known patterns | Sonnet | Mechanical, tests verify |
| Document formatting | Sonnet/Haiku | No judgment required |
| Contract review / redlines | Opus | Judgment-heavy, costly if wrong |
| Novel decisions / architecture | Opus | Ambiguous context, stakes high |
| New skill / workflow creation | Opus | Requires architectural judgment |
| Analysis going to external party | Opus | Reputation cost if shallow |
| Simple lookups | Haiku/Sonnet | Single source, no synthesis |

---

## Confirmed Wins (positive signal → tier was right)

| Date | Task | Model Used | Signal |
|------|------|-----------|--------|
| — | — | — | — |

---

## Escalation Events (output too shallow → should have been Opus)

| Date | Task | Model Used | Signal | Action Taken |
|------|------|-----------|--------|-------------|
| — | — | — | — | — |

---

## De-escalation Events (output too slow/heavy → should have been Sonnet)

| Date | Task | Model Used | Signal | Action Taken |
|------|------|-----------|--------|-------------|
| — | — | — | — | — |

---

## Real-Time Action Protocol

Signals fire **in the moment**, not at end of session.

**On positive confirmation** ("excellent", "this is great", "perfect", "exactly what I needed"):
1. Acknowledge briefly what worked ("Glad that landed — Sonnet on the report, noted")
2. Log to Confirmed Wins table above
3. If task type isn't in Confirmed Tier Assignments, add it

**On escalation signal** ("not in depth enough", "you're not understanding", "too shallow", "you're missing the point"):
1. Acknowledge briefly ("Got it — going deeper on this")
2. Switch to Opus for the rest of the session immediately
3. Log to Escalation Events table above
4. If same task type escalates 2+ times → update Confirmed Tier Assignments and CLAUDE.md default

**On de-escalation signal** ("that took way too long", "just do it", "you're overthinking it"):
1. Acknowledge ("Got it — keeping it tighter")
2. Adjust approach for the rest of the session
3. Log to De-escalation Events table above
4. If same task type de-escalates 2+ times → update Confirmed Tier Assignments and CLAUDE.md default

---

## Signal Reference

### Positive confirmation (tier was right)
- "excellent"
- "this is great"
- "perfect"
- "exactly what I needed"
- "that's it"
- "love it"
- "good work"
- "well done"
- User forwards or uses the output immediately without edits

### Escalation (too shallow — go deeper / switch to Opus)
- "that's not in depth enough"
- "you're not understanding"
- "you're missing the point"
- "this isn't what I needed"
- "too shallow"
- "you're not getting it"
- "I need more than that"
- "dig deeper"
- Asking the same question a second time after an unsatisfying answer
- Correcting a factual or analytical error mid-session

### De-escalation (too heavy — go faster / switch to Sonnet)
- "that took way too long"
- "why is this taking so long"
- "just do it"
- "keep it simple"
- "I don't need all of this"
- "too much"
- "you're overthinking it"
- "just run the skill"
- "this should be fast"
