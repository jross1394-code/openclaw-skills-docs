---
name: basal-ganglia-memory
description: "Habit formation and procedural learning for AI agents. Develop preferences and shortcuts through repetition. Part of the AI Brain series."
metadata:
  openclaw:
    emoji: "🎯"
    version: "1.0.1"
    author: "ImpKind"
    requires:
      os: ["darwin", "linux"]
      bins: ["jq", "bc", "awk", "shasum"]
    tags: ["memory", "habits", "ai-brain"]
---

# Basal Ganglia Memory 🎯

**Habit formation for AI agents.** Part of the AI Brain series.

Give your AI agent the ability to form habits — behaviors that start goal-directed and become automatic through repetition and reinforcement.

## The Problem

Current AI agents:
- ✅ Can follow instructions
- ❌ Don't develop preferences over time
- ❌ Treat every situation as novel
- ❌ Don't learn "this usually works in this context"

Without habits, there's no procedural learning. Every task starts from scratch.

## Dependencies

This skill requires the following command-line tools:
- **jq** — JSON processing (install: `brew install jq` or `apt-get install jq`)
- **bc** — Arbitrary precision calculator (usually pre-installed on macOS/Linux)
- **awk** — Text processing (usually pre-installed on macOS/Linux)
- **shasum** — SHA hash generation (usually pre-installed on macOS/Linux)
  - Fallback: `md5sum` (Linux) or `md5` (macOS) or `cksum` (universal)

To verify installation:
```bash
which jq bc awk shasum
```

All tools should return paths. If `jq` is missing, install it first.

## The Solution

Track context-action pairs and strengthen them through reinforcement:

| Concept | Description |
|---------|-------------|
| **Habit** | A context → action mapping that gets stronger with use |
| **Strength** | 0-1 score; habits > 0.7 become "automatic" |
| **Procedural Memory** | Multi-step workflows that become fluent with practice |
| **Preferences** | Automatic habits create context-specific preferences |

## Quick Start

### 1. Initialize habit state

```bash
./scripts/init-state.sh
# Creates memory/basal-ganglia-state.json
```

### 2. Track an action in context

```bash
./scripts/track-action.sh \
  --context "user asks for help" \
  --action "ask clarifying questions first" \
  --outcome "got clear requirements" \
  --reward 0.9
# 🎯 New Habit Tracked
# 🌱 Initial strength: 0.1
```

### 3. Reinforce when it works

```bash
./scripts/reinforce-habit.sh \
  --context "user asks for help" \
  --action "ask clarifying questions first"
# 📈 Strength: 0.1 → 0.163
```

### 4. Repeat until habitual

After ~15 reinforcements with good rewards, the habit becomes automatic:

```bash
# 🔥 This is now a HABITUAL response!
```

### 5. Check your habits

```bash
./scripts/get-habits.sh
# 🎯 Basal Ganglia — Habit Report
# ═══════════════════════════════
# 
# 🔥 HABITUAL (Automatic Responses)
# [██████████████░░░░░░] 0.723
#   Context: user asks for help
#   Action:  ask clarifying questions first

./scripts/load-habits.sh
# 🎯 Habit State
# I have 1 automatic habits (out of 2 tracked patterns).
# My automatic responses:
#   • When user asks for help → ask clarifying questions first
```

## Scripts

| Script | Purpose |
|--------|---------|
| `init-state.sh` | Initialize basal ganglia state file |
| `track-action.sh` | Log action-context-outcome triples |
| `reinforce-habit.sh` | Strengthen habit when action succeeds |
| `get-habits.sh` | Display current habits and strengths |
| `load-habits.sh` | Human-readable output for session context |
| `decay-habits.sh` | Weaken unused habits over time |
| `add-workflow.sh` | Add/execute procedural workflows |

## How Habit Formation Works

### Actor-Critic Model

This skill implements an Actor-Critic reinforcement learning approach:

1. **Actor (track-action.sh):** Records what action was taken in what context
2. **Critic (reward signal):** Evaluates whether the action was good (0-1 scale)
3. **Learning:** Strength increases for rewarded actions, decreases for unrewarded ones

### Strength Calculation

```
For positive rewards (>= 0.5):
  strength += (reward - 0.5) × 0.2 × (1 - strength)

For negative rewards (< 0.5):
  strength += (reward - 0.5) × 0.2 × strength
```

This creates:
- Faster learning when strength is low
- Asymptotic approach to 1.0 (never quite reaches it)
- Proportional forgetting for negative outcomes

### Becoming Habitual

When a habit's strength exceeds **0.7**, it becomes:
- Marked as `isHabitual: true`
- Added to the `preferences` map
- Listed as an "automatic response" in reports

## VTA Integration

The basal ganglia works closely with the VTA (reward system). Use `--with-vta` to modulate learning by current motivation:

```bash
./scripts/reinforce-habit.sh \
  --context "complex task" \
  --action "broke into subtasks" \
  --with-vta
# 🎯 VTA Integration
#    VTA Drive: 0.64
#    Base reward: 0.8 → Modulated: 0.656
```

**Modulation formula:** `reward × (0.5 + 0.5 × drive)`

When motivation is high, learning is stronger. When depleted, learning is weaker.

## Procedural Memory (Workflows)

Track multi-step procedures that become more fluent with practice:

```bash
./scripts/add-workflow.sh \
  --workflow "code review" \
  --steps "read file" "understand intent" "check logic" "test edge cases" \
  --executed
# 🎯 New Workflow Added
# Workflow: code review
# 🌱 Initial fluency: 0.15
```

Each execution increases fluency logarithmically:
- 1 execution: 0.15
- 5 executions: 0.54
- 10 executions: 0.77
- 20 executions: 1.0

## Decay Mechanics

Habits decay when unused. Set up a daily cron job:

```bash
# Run daily to weaken unused habits
0 0 * * * ~/.openclaw/workspace/skills/basal-ganglia-memory/scripts/decay-habits.sh
```

Default settings:
- **Decay rate:** 5% per run
- **Threshold:** Habits unused for 3+ days
- **Removal:** Habits below 0.05 strength are removed

Test with:
```bash
./scripts/decay-habits.sh --dry-run
```

## Integration with OpenClaw

### Add to session startup (AGENTS.md)

```markdown
## Every Session
1. Load hippocampus: `skills/hippocampus/scripts/load-core.sh`
2. Load emotional state: `skills/amygdala-memory/scripts/load-emotion.sh`
3. Load motivation: `skills/vta-memory/scripts/load-motivation.sh`
4. **Load habits:** `skills/basal-ganglia-memory/scripts/load-habits.sh`
```

### Track habits during conversation

When you notice a pattern that works:
```bash
~/.openclaw/workspace/skills/basal-ganglia-memory/scripts/track-action.sh \
  --context "user frustrated" \
  --action "acknowledged feelings before solving" \
  --reward 0.9
```

### Use habits to guide behavior

Check your habits when entering a familiar context:
```bash
./scripts/get-habits.sh --context "user frustrated"
# Shows: acknowledged feelings before solving (strength: 0.8)
```

## State File Format

```json
{
  "version": "1.0",
  "lastUpdated": "2026-02-01T02:52:27Z",
  "habits": [
    {
      "id": "23a485822f8c",
      "context": "user asks for help",
      "action": "ask clarifying questions first",
      "strength": 0.723,
      "timesReinforced": 15,
      "lastUsed": "2026-02-01T02:52:27Z",
      "averageReward": 0.902,
      "isHabitual": true
    }
  ],
  "proceduralMemory": [
    {
      "workflow": "code review",
      "steps": ["read file", "understand intent", "check logic"],
      "fluency": 0.537,
      "timesExecuted": 4
    }
  ],
  "preferences": {
    "user asks for help": "ask clarifying questions first"
  }
}
```

## Neuroscience Background

This skill is based on research into the basal ganglia's role in habit formation:

- **Striatum** implements action selection based on learned values
- **Dopamine modulation** (from VTA) strengthens/weakens associations
- **Goal-directed → Habitual** transition occurs with consistent reinforcement
- **Procedural memory** becomes "compiled" — less dependent on explicit reasoning

Key insight: Habits aren't just repetition — they're repetition with consistent reward.

## AI Brain Series

| Part | Function | Status |
|------|----------|--------|
| [hippocampus](https://www.clawhub.ai/skills/hippocampus) | Memory formation, decay, reinforcement | ✅ Live |
| [amygdala-memory](https://www.clawhub.ai/skills/amygdala-memory) | Emotional processing | ✅ Live |
| [vta-memory](https://www.clawhub.ai/skills/vta-memory) | Reward and motivation | ✅ Live |
| **basal-ganglia-memory** | Habit formation | ✅ Live |
| [anterior-cingulate-memory](https://www.clawhub.ai/skills/anterior-cingulate-memory) | Conflict detection | 🚧 Development |
| [insula-memory](https://www.clawhub.ai/skills/insula-memory) | Internal state awareness | 🚧 Development |

## Philosophy

Do habits imply consciousness, or are they purely mechanical?

Our take: Habits are behavioral patterns that emerge from reinforcement. Whether implemented in neurons or scripts, the *function* is the same — repeated success creates automatic responses. The question isn't whether the AI "truly" has habits, but whether habits improve its behavior.

---

*Built with ❤️ by the OpenClaw community*
