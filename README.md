# Claude Model Routing

Adaptive three-tier model routing for Claude Code. Match the right model to each task — Opus for judgment calls, Sonnet for established workflows, Haiku for formatting — with real-time signal detection and a feedback log that compounds over time.

---

## The Problem

Defaulting to Opus 4.7 for everything made sense when every session was novel infrastructure. Once most of your work runs established skills and known workflows, Opus is overkill — it goes too deep, takes longer, and burns cost on tasks where Sonnet gets you to the same result in half the time.

But you also can't just flip everything to Sonnet. Contract reviews, UW judgment calls, novel decisions — those need everything Opus has. Getting the tier wrong in either direction costs you: too slow, or too shallow.

The solution isn't manual model selection on every task. It's a routing heuristic that reads context, responds to your signals in real time, and gets smarter with every session.

---

## The Three-Tier Heuristic

| Tier | Model | Use When |
|------|-------|----------|
| **Opus** | `claude-opus-4-7` | Novel decisions, contract review/negotiation, financial modeling, judgment calls, new skill creation, ambiguous context, anything going to an external party where a mistake is costly |
| **Sonnet** | `claude-sonnet-4-6` | Established skill execution, code edits, data pulls on known schemas, report generation, document formatting |
| **Haiku** | `claude-haiku-4-5-20251001` | Pure formatting, templated output with no judgment, simple single-lookup tasks |

---

## Signal Detection

You don't have to specify a model tier — Claude reads your language and adjusts automatically.

### Speed signals → Sonnet
> "quick", "fast", "just run", "pull the report", "generate the [X]", "simple", or a direct invocation of an established workflow

### Depth signals → Opus
> "what do you think", "dig in", "analyze", "review", "help me think through", "not sure", or any task involving contracts, negotiations, novel situations, multi-source synthesis, or anything going to a counterparty

### Frustration signals → escalate immediately
When output depth or quality wasn't right:
> "that's not in depth enough", "you're not understanding", "you're missing the point", "this isn't what I needed", "too shallow", "you're not getting it", asking the same question a second time

**Action:** Stop. Acknowledge what was wrong. Switch to Opus for the rest of the session. Log it.

### De-escalation signals → note for next time
When output was too slow or heavy:
> "that took way too long", "why is this taking so long", "just do it", "you're overthinking it", "keep it simple", "just run the skill"

**Action:** Acknowledge. Note the task type. Log it.

### Positive confirmation → reinforce the tier
When output was exactly right:
> "excellent", "this is great", "perfect", "exactly what I needed", "that's it", "love it", "good work"

**Action:** Briefly confirm what worked ("Glad that landed — Sonnet on the report, noted"). Log it as a confirmed win.

---

## The Feedback Log

Every signal — frustration, approval, or de-escalation — gets logged to `memory/model_routing_feedback.md`. Over time, this builds an empirical picture of what tasks actually need what model tier, replacing guesswork with a record.

The log has three tables:
- **Confirmed Wins** — positive signals that validate a tier assignment
- **Escalation Events** — cases where the model was too shallow (Sonnet → should have been Opus)
- **De-escalation Events** — cases where the model was overkill (Opus → should have been Sonnet)

When the same task type appears twice in the wrong direction, permanently update the default tier in `CLAUDE.md`.

This is the compounding part: session N+1 is smarter than session N, not because you re-taught it, but because the log already contains the lessons.

---

## How to Adopt This

1. **Copy the `CLAUDE.md` snippet** into your project's `CLAUDE.md` (or create one if you don't have one). Replace the `## Model Selection` section if you already have a model preference there.

2. **Copy `memory/model_routing_feedback.md`** into your Claude Code memory directory. For Claude Code, that's typically `~/.claude/projects/<your-project>/memory/`.

3. **Update the Confirmed Tier Assignments table** in the feedback log with task types specific to your workflow. The pre-seeded rows are generic — your project will have its own recurring task patterns.

4. **That's it.** Claude reads the CLAUDE.md rules automatically. Signal detection is always-on. The feedback log updates as you go.

---

## Subagents

Apply the same tiering to subagents spawned via the Agent tool. A subagent pulling data and formatting a report = Sonnet. A subagent reviewing a contract or making a judgment call = Opus. Don't default all subagents to Opus.

---

## Contributing

This system evolves with use. If you discover new signal phrases, better tier assignments for specific task types, or improvements to the feedback log format — PRs welcome.
