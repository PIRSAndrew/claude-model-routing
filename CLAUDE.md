# Claude Code — Model Routing

Drop this `## Model Selection` section into your project's `CLAUDE.md`. It replaces any blanket "always use Opus" or "always use Sonnet" rules.

---

## Model Selection

Three-tier routing — match the model to the task, not to maximum safety.

| Tier | Model | Use When |
|------|-------|----------|
| **Opus 4.7** | `claude-opus-4-7` | Novel decisions, contract review/negotiation, financial modeling, judgment calls, new skill creation, ambiguous context, anything going to an external party where a mistake is costly |
| **Sonnet 4.6** | `claude-sonnet-4-6` | Established skill execution, code edits, data pulls on known schemas, report generation, document formatting |
| **Haiku 4.5** | `claude-haiku-4-5-20251001` | Pure formatting, templated output with no judgment, simple single-lookup tasks |

### Speed signals → Sonnet
User says: "quick", "fast", "just run", "pull the report", "generate the [X]", "simple", or directly invokes an established workflow with known output format.

### Depth signals → Opus
User says: "what do you think", "dig in", "analyze", "review", "help me think through", "not sure", or the task involves contracts, negotiations, novel situations, multi-source synthesis, anything going to a counterparty.

### Frustration escalation rule (real-time, not end-of-session)
When the user signals dissatisfaction mid-task, act immediately — don't finish the response at the wrong depth:
1. Acknowledge briefly ("Got it — going deeper on this")
2. Switch to Opus for the remainder of the session
3. Log the task type and phrase to `memory/model_routing_feedback.md`

**Escalation signals** (output was too shallow/wrong): "that's not in depth enough", "you're not understanding", "you're missing the point", "this isn't what I needed", "too shallow", "you're not getting it", asking the same question a second time

**De-escalation signals** (output was too slow/heavy): "that took way too long", "why is this taking so long", "just do it", "you're overthinking it", "keep it simple"

**Positive confirmation signals** (model + approach were right): "excellent", "this is great", "perfect", "exactly what I needed", "that's it", "love it", "good work" — briefly acknowledge what worked and log to `memory/model_routing_feedback.md` Confirmed Wins table.

### Subagents
Apply the same tiering to subagents. A subagent pulling data and formatting = Sonnet. A subagent reviewing a contract or making a judgment call = Opus. Don't default all subagents to Opus.
