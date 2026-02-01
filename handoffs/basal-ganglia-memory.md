# Basal Ganglia Memory Implementation Handoff

**Completed:** 2026-02-01  
**Implementer:** Echo (subagent)  
**Status:** ✅ Complete and tested

---

## What Was Implemented

### Scripts Created (`skills/basal-ganglia-memory/scripts/`)

| Script | Purpose | Tested |
|--------|---------|--------|
| `init-state.sh` | Initialize basal ganglia state at `memory/basal-ganglia-state.json` | ✅ |
| `track-action.sh` | Log action-context-outcome triples, create/update habits | ✅ |
| `reinforce-habit.sh` | Convenience wrapper for positive reinforcement | ✅ |
| `get-habits.sh` | Display current habits, workflows, and preferences | ✅ |
| `load-habits.sh` | Human-readable output for session context | ✅ |
| `decay-habits.sh` | Weaken unused habits over time | ✅ |
| `add-workflow.sh` | Add/execute procedural memory workflows | ✅ |

### State File (`memory/basal-ganglia-state.json`)

```json
{
  "version": "1.0",
  "lastUpdated": "ISO-8601",
  "habits": [
    {
      "id": "hash-12-chars",
      "context": "when X situation",
      "action": "do Y",
      "strength": 0.0-1.0,
      "timesReinforced": int,
      "lastUsed": "ISO-8601",
      "averageReward": 0.0-1.0,
      "isHabitual": bool
    }
  ],
  "proceduralMemory": [
    {
      "workflow": "task name",
      "steps": ["step1", "step2"],
      "fluency": 0.0-1.0,
      "timesExecuted": int
    }
  ],
  "preferences": {
    "context": "preferred-action"
  },
  "actionHistory": []
}
```

### Key Mechanics

| Mechanic | Details |
|----------|---------|
| **Habit Threshold** | Strength > 0.7 = habitual (automatic) |
| **Strength Formula** | `strength += (reward - 0.5) × 0.2 × (1 - strength)` for positive |
| **VTA Integration** | `--with-vta` modulates reward by motivation drive |
| **Decay Rate** | 5% per run for habits unused 3+ days |
| **Fluency Formula** | `fluency = log(executions+1) / log(20)` |

---

## How to Use

### Session Startup

Add to AGENTS.md under "Every Session":

```markdown
5. **Load habits:** `skills/basal-ganglia-memory/scripts/load-habits.sh`
```

### During Conversations

Track actions when patterns emerge:
```bash
skills/basal-ganglia-memory/scripts/track-action.sh \
  --context "user frustrated" \
  --action "acknowledged feelings first" \
  --reward 0.9
```

Reinforce successful patterns:
```bash
skills/basal-ganglia-memory/scripts/reinforce-habit.sh \
  --context "user frustrated" \
  --action "acknowledged feelings first" \
  --with-vta  # Use VTA drive as reward multiplier
```

### Check State

Quick check:
```bash
skills/basal-ganglia-memory/scripts/load-habits.sh --format brief
# Habits: 2 total, 1 automatic | Top: ask clarifying questions first | Prefs: 1
```

Full report:
```bash
skills/basal-ganglia-memory/scripts/get-habits.sh
```

---

## Cron Job Recommendation

Set up decay to run daily:

```bash
0 0 * * * ~/.openclaw/workspace/skills/basal-ganglia-memory/scripts/decay-habits.sh
```

Or add to HEARTBEAT.md for periodic decay during heartbeat cycles.

---

## Integration Points

### With VTA Memory (Reward System)

The basal ganglia uses VTA's reward signals:
- `--with-vta` flag modulates reinforcement by current drive
- High motivation = stronger learning
- Low motivation = weaker learning

### With Hippocampus (Memory)

Context recognition comes from episodic memory:
- Habits reference contexts that should match hippocampus memories
- Could add automatic reinforcement of context memories when habits fire

### With Amygdala (Emotions)

Emotional context affects habit appropriateness:
- Consider emotional state when suggesting habitual actions
- Some habits may only apply in certain emotional contexts

---

## Next Steps: Anterior Cingulate Memory (Conflict Detection)

The Anterior Cingulate skill should:

1. **Detect conflicts** between:
   - Multiple applicable habits
   - Habit vs explicit instruction
   - Expected vs actual outcomes

2. **Monitor errors** when:
   - Habitual response leads to bad outcome
   - User corrects agent behavior
   - Predictions don't match reality

3. **Use habit data** to:
   - Flag when multiple habits could apply (strength > 0.5)
   - Monitor when habitual responses get low rewards
   - Suggest "slow down" when in unfamiliar territory

### Suggested ACC State Schema

```json
{
  "version": "1.0",
  "lastUpdated": "ISO-8601",
  "recentConflicts": [
    {
      "id": "conflict-id",
      "type": "habit-conflict|expectation-violation|error",
      "context": "what situation",
      "competing": ["option1", "option2"],
      "resolution": "what was chosen",
      "outcome": "success|failure",
      "timestamp": "ISO-8601"
    }
  ],
  "errorHistory": [
    {
      "error": "what went wrong",
      "context": "when",
      "correction": "what should have happened",
      "learned": "lesson extracted"
    }
  ],
  "uncertaintyThreshold": 0.3,
  "errorLikelihood": {
    "context": 0.0-1.0
  }
}
```

### ACC Integration with Basal Ganglia

```bash
# ACC queries habits for conflict detection
habits=$(./skills/basal-ganglia-memory/scripts/get-habits.sh --json --context "current situation")

# If multiple habits apply with similar strength → conflict!
competing=$(echo "$habits" | jq '[.[] | select(.strength > 0.5)] | length')
if [ "$competing" -gt 1 ]; then
  # Log conflict, request deliberation
fi
```

---

## Files Modified/Created

| File | Action |
|------|--------|
| `skills/basal-ganglia-memory/scripts/init-state.sh` | Created |
| `skills/basal-ganglia-memory/scripts/track-action.sh` | Created |
| `skills/basal-ganglia-memory/scripts/reinforce-habit.sh` | Created |
| `skills/basal-ganglia-memory/scripts/get-habits.sh` | Created |
| `skills/basal-ganglia-memory/scripts/load-habits.sh` | Created |
| `skills/basal-ganglia-memory/scripts/decay-habits.sh` | Created |
| `skills/basal-ganglia-memory/scripts/add-workflow.sh` | Created |
| `skills/basal-ganglia-memory/SKILL.md` | Updated (was placeholder) |
| `memory/basal-ganglia-state.json` | Created (during testing) |
| `memory/basal-ganglia-log.jsonl` | Created (action history) |
| `projects/AI_BRAIN_IMPLEMENTATION.md` | Updated (marked BG ✅) |
| `projects/handoffs/basal-ganglia-memory.md` | Created (this file) |

---

## Testing Summary

All scripts tested and working:

1. ✅ `init-state.sh` — Creates valid JSON, --force works
2. ✅ `track-action.sh` — Creates new habits, updates existing ones
3. ✅ `track-action.sh` — Habit IDs use shasum for consistency
4. ✅ `reinforce-habit.sh` — Calls track-action with positive reward
5. ✅ `reinforce-habit.sh --with-vta` — Modulates by VTA drive
6. ✅ `get-habits.sh` — Full display, --json, --habitual-only modes
7. ✅ `load-habits.sh` — Prose, brief, and JSON formats
8. ✅ `decay-habits.sh` — Correctly identifies stale habits, --dry-run works
9. ✅ `add-workflow.sh` — Creates and executes workflows
10. ✅ Habit formation — Strength increases to 0.7+ after ~15 reinforcements
11. ✅ Automatic preferences — Habitual habits create preferences map

---

## Example Session

```bash
# Initialize
./scripts/init-state.sh

# Track a successful pattern
./scripts/track-action.sh \
  --context "user asks for help" \
  --action "ask clarifying questions first" \
  --reward 0.9

# Reinforce 15 times over several sessions...
./scripts/reinforce-habit.sh \
  --context "user asks for help" \
  --action "ask clarifying questions first"

# Check results
./scripts/load-habits.sh
# 🎯 Habit State
# I have 1 automatic habits (out of 1 tracked patterns).
# My automatic responses:
#   • When user asks for help → ask clarifying questions first
```

---

*Handoff complete. Ready for Anterior Cingulate implementation.*
