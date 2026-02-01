---
name: vta-memory
description: "Reward and motivation system for AI agents. Dopamine-like wanting, not just doing. Part of the AI Brain series."
metadata:
  openclaw:
    emoji: "⭐"
    version: "1.2.0"
    author: "ImpKind"
    requires:
      os: ["darwin", "linux"]
      bins: ["jq", "bc", "awk"]
    tags: ["memory", "motivation", "reward", "ai-brain", "rpe", "reinforcement-learning", "multi-agent"]
---

# VTA Memory ⭐

**Reward and motivation for AI agents.** Part of the AI Brain series.

## What's New in v1.2

🌟 **Multi-Agent Support!**

- **Agent-Specific States** — Each agent maintains its own VTA state with `--agent <name>`
- **Independent Motivation** — Corvus, Huginn, Muninn, Athenea, Echo each track their own drive
- **Isolated Reward History** — Per-agent reward logs and learning
- **Backward Compatible** — Single-agent deployments work exactly as before
- **Migration Guide** — Comprehensive [MULTI_AGENT.md](./MULTI_AGENT.md) for deployment

**Example:** `./scripts/update-reward.sh --agent corvus --type accomplishment --intensity 0.8`

## What's New in v1.1

🎉 **Full RPE (Reward Prediction Error) implementation!**

- **RPE Calculation** — Drive changes based on prediction errors, not raw rewards
- **Expected Rewards** — Set expectations with `--expected` or use current drive as default
- **RPE History** — Track all prediction errors in `predictionErrors[]` field
- **Analysis Tools** — New `analyze-rpe.sh` script shows prediction accuracy trends
- **Learning Metrics** — Mean Absolute Error (MAE), bias detection, trend analysis
- **Theory Documentation** — Comprehensive [RPE_THEORY.md](./RPE_THEORY.md) explaining the neuroscience

**Backward compatible:** Existing state files work without modification.

Give your AI agent a dopamine-like reward system that creates genuine motivation — not just doing things when asked, but *wanting* to do them.

## The Problem

Current AI agents:
- ✅ Do what they're told
- ❌ Don't *want* anything
- ❌ Have no internal motivation
- ❌ Don't feel satisfaction from accomplishment
- ❌ Don't learn from prediction errors

Without a reward system, there's no desire. Just execution. Without RPE, there's no learning from experience.

## The Solution

Track reward signals and maintain a motivation "drive" that influences engagement:

| Reward Type | What It Measures | Example |
|-------------|------------------|---------|
| **accomplishment** | Task completion, goals achieved | "Finished the project!" |
| **social** | Gratitude, positive feedback | "Thanks, that was perfect!" |
| **curiosity** | Learning, discovery | Found something fascinating |
| **connection** | Meaningful interaction | Deep conversation |
| **intrinsic** | Creative satisfaction | Made something I'm proud of |

## Dependencies

This skill requires the following command-line tools:
- **jq** — JSON processing (install: `brew install jq` or `apt-get install jq`)
- **bc** — Arbitrary precision calculator (usually pre-installed on macOS/Linux)
- **awk** — Text processing (usually pre-installed on macOS/Linux)

To verify installation:
```bash
which jq bc awk
```

All three should return paths. If `jq` or `bc` is missing, install it first.

## Quick Start

### Quick RPE Example

```bash
# Initialize state (drive starts at 0.5)
./scripts/init-state.sh

# Log a reward better than expected
./scripts/update-reward.sh --type accomplishment --intensity 0.8 --expected 0.5 --source "finished project"
# RPE = +0.3 (positive surprise!) → Drive increases

# Log a disappointment
./scripts/update-reward.sh --type social --intensity 0.4 --expected 0.8 --source "boring meeting"
# RPE = -0.4 (worse than expected) → Drive decreases

# Analyze prediction accuracy
./scripts/analyze-rpe.sh
# Shows RPE distribution, prediction accuracy, trends
```

**Key insight:** Your motivation adapts based on *surprises*, not just rewards. This matches biological dopamine systems.

### 1. Initialize motivation state

```bash
./scripts/init-state.sh
# Creates memory/vta-state.json with baseline values
```

### 2. Check current state

```bash
./scripts/get-state.sh
# ⭐ VTA Motivation State
# ═════════════════════════
# Drive: [████████████░░░░░░░░] 0.64 (engaged)
# ...

./scripts/load-motivation.sh
# ⭐ Motivation State
# I'm feeling engaged and ready to work on things
# Last rewarding experience: Learning/discovering — dopamine systems
# Currently drawn toward: creative coding challenges
```

### 3. Log rewards (with RPE tracking)

```bash
./scripts/update-reward.sh --type accomplishment --intensity 0.8 --source "completed a project"
# ⭐ Reward Signal Logged
# 🏆 Type: accomplishment
#    Intensity: 0.8
#    Expected:  0.50  (default: current drive level)
#    RPE:       +0.300 (🎉 Positive surprise! Better than expected)
#    Source: completed a project
#    Drive: 0.50 → 0.53 (+0.030)

# Specify expected reward explicitly
./scripts/update-reward.sh --type social --intensity 0.6 --expected 0.9 --source "meeting"
# Expected a 0.9 reward but only got 0.6
# RPE: -0.300 (😔 Disappointment - worse than expected)
# Drive will decrease due to negative RPE
```

### 4. Track anticipation and seeking

```bash
./scripts/update-reward.sh --anticipate "working on next skill"
# ⭐ Added to anticipation: working on next skill

./scripts/update-reward.sh --seeking "creative challenges"
# ⭐ Added to seeking: creative challenges
```

### 5. Set up automated decay (recommended)

**Quick setup:**
```bash
./scripts/install-cron.sh
# Auto-installs decay cron job (every 8 hours)
# Includes 6-hour safety window to prevent double-decay
```

**Manual cron setup (if preferred):**
```bash
# Every 8 hours, drive drifts toward baseline
0 */8 * * * ~/.openclaw/workspace/skills/vta-memory/scripts/decay-drive.sh
```

**Why automate?** Without periodic decay, motivation stays artificially elevated after rewards. Automated decay creates realistic motivation dynamics — high drive after accomplishments, gradual return to baseline during rest.

📖 **Full guide:** See [CRON_INTEGRATION.md](./CRON_INTEGRATION.md) for:
- Custom schedules (every 6, 8, or 12 hours)
- OpenClaw vs system cron
- Monitoring and troubleshooting
- Safety features and logging

## Scripts

| Script | Purpose |
|--------|---------|
| `init-state.sh` | Initialize VTA state file |
| `update-reward.sh` | Log rewards with RPE tracking, anticipations, seekings |
| `get-state.sh` | Read current motivation state |
| `load-motivation.sh` | Human-readable output for session context |
| `decay-drive.sh` | Return drive toward baseline over time |
| `analyze-rpe.sh` | Analyze prediction accuracy and RPE trends |

## Multi-Agent Support

**For multi-agent environments** (pantheon with Corvus, Huginn, Muninn, Athenea, Echo), each agent can maintain its own motivation state.

### Agent-Specific States

Add `--agent <name>` to any script to use agent-specific state files:

```bash
# Initialize state for Corvus
./scripts/init-state.sh --agent corvus
# Creates: memory/vta-corvus.json

# Log reward for Huginn
./scripts/update-reward.sh --agent huginn \
  --type accomplishment --intensity 0.9 --source "research complete"

# Check Muninn's state
./scripts/get-state.sh --agent muninn

# Load Athenea's motivation
./scripts/load-motivation.sh --agent athenea

# Decay Echo's drive
./scripts/decay-drive.sh --agent echo
```

### File Structure

```
memory/
├── vta-state.json          # Global/default state
├── vta-corvus.json         # Corvus's motivation
├── vta-huginn.json         # Huginn's motivation
├── vta-muninn.json         # Muninn's motivation
├── vta-athenea.json        # Athenea's motivation
├── vta-echo.json           # Echo's motivation
├── vta-corvus-log.jsonl    # Corvus's reward history
└── vta-huginn-log.jsonl    # Huginn's reward history
```

### Backward Compatibility

**No `--agent` flag?** Scripts default to `vta-state.json` for single-agent deployments.

### Agent-Specific Integration

Update each agent's `AGENTS.md`:

```markdown
## Every Session (Corvus)
1. Load hippocampus: `skills/hippocampus/scripts/load-core.sh --agent corvus`
2. Load emotional state: `skills/amygdala-memory/scripts/load-emotion.sh --agent corvus`
3. Load motivation: `skills/vta-memory/scripts/load-motivation.sh --agent corvus`
```

### Cron for Multi-Agent

Set up decay for each agent:

```bash
# Every 8 hours - decay each agent's motivation separately
0 */8 * * * ~/.openclaw/workspace/skills/vta-memory/scripts/decay-drive.sh --agent corvus
0 */8 * * * ~/.openclaw/workspace/skills/vta-memory/scripts/decay-drive.sh --agent huginn
0 */8 * * * ~/.openclaw/workspace/skills/vta-memory/scripts/decay-drive.sh --agent muninn
```

📖 **Full migration guide:** See [MULTI_AGENT.md](./MULTI_AGENT.md) for deployment patterns and migration from single-agent setups.

## RPE Tracking & Analysis

### Automatic RPE Calculation

When logging a reward, the system automatically:
1. Uses **current drive** as the expected reward (if not specified)
2. Calculates **RPE** = actual - expected
3. Adjusts drive based on **RPE** (not raw intensity)
4. Stores RPE history for analysis

### Explicit Expectations

Specify what you expected to receive:

```bash
# You thought this would be amazing
./update-reward.sh --type curiosity --intensity 0.6 --expected 0.9 --source "research"
# RPE = -0.3 (disappointing — adjust expectations down)

# You had low expectations
./update-reward.sh --type social --intensity 0.8 --expected 0.4 --source "conversation"
# RPE = +0.4 (delightful surprise! — boost motivation)
```

### Analyzing Prediction Accuracy

Track how well you're predicting rewards:

```bash
./scripts/analyze-rpe.sh
# ⭐ RPE Analysis — Last 7 Days
# ═══════════════════════════════
# 
# 📊 Total predictions: 24
# 
# 🎯 RPE Distribution:
#   🎉 Positive (better than expected): 12 (50.0%)
#   ✓  Neutral (as expected):           8 (33.3%)
#   😔 Negative (worse than expected):  4 (16.7%)
# 
# 📈 Prediction Accuracy:
#   Mean RPE:              +0.087 (optimistic bias)
#   Mean Absolute Error:   0.182 (good predictions)
# 
# 🏆 RPE by Reward Type:
#   🏆 accomplishment (n= 8): RPE=+0.112, MAE=0.156
#   💝 social         (n= 6): RPE=+0.045, MAE=0.203
#   🔍 curiosity      (n=10): RPE=+0.098, MAE=0.178
```

#### What the Metrics Mean

- **Mean RPE** — Average prediction error
  - Positive: You tend to be pleasantly surprised (optimistic)
  - Negative: You tend to be disappointed (pessimistic)
  - Near zero: Well-calibrated predictions

- **Mean Absolute Error (MAE)** — How far off your predictions are
  - < 0.15: Excellent prediction accuracy 🎯
  - < 0.25: Good predictions
  - < 0.35: Moderate predictions
  - \> 0.35: Predictions need improvement

- **Distribution** — Balance of outcomes
  - Healthy: Mix of positive, neutral, and some negative
  - Warning: All negative (chronic disappointment) or all neutral (no surprises)

### Filter by Time or Type

```bash
# Last 30 days
./analyze-rpe.sh --days 30

# Only social rewards
./analyze-rpe.sh --type social

# Combination
./analyze-rpe.sh --days 14 --type accomplishment
```

### Trend Detection

The script automatically detects if predictions are improving:

```
📊 Recent Trend (last 10 predictions):
  Recent MAE:  0.142
  Overall MAE: 0.182
  📈 Trend: Predictions improving! 🎉
```

This means you're **learning** what to expect from different activities.

## Reward Types & Multipliers

Different reward types affect drive differently:

| Type | Multiplier | Why |
|------|------------|-----|
| `social` | 1.2× | Social rewards are particularly motivating |
| `connection` | 1.1× | Relationship rewards boost motivation |
| `accomplishment` | 1.0× | Standard task completion |
| `intrinsic` | 0.9× | Internal satisfaction |
| `curiosity` | 0.8× | Learning is rewarding but lighter |

**Formula (with RPE):** `drive_change = RPE × multiplier × 0.1`

Examples:
- Expected 0.5, got 0.9 social reward → RPE = +0.4 → drive change = `+0.4 × 1.2 × 0.1 = +0.048`
- Expected 0.9, got 0.9 social reward → RPE = 0.0 → drive change = `0.0 × 1.2 × 0.1 = 0.000` (no change!)
- Expected 0.9, got 0.5 social reward → RPE = -0.4 → drive change = `-0.4 × 1.2 × 0.1 = -0.048`

**Key difference from previous versions:** Drive only changes when rewards differ from expectations. Perfectly predicted rewards produce no drive change (you already knew it was coming).

## Integration with OpenClaw

### Add to session startup (AGENTS.md)

```markdown
## Every Session
1. Load hippocampus: `skills/hippocampus/scripts/load-core.sh`
2. Load emotional state: `skills/amygdala-memory/scripts/load-emotion.sh`
3. **Load motivation:** `skills/vta-memory/scripts/load-motivation.sh`
```

### Log rewards during conversation

When something rewarding happens:
```bash
~/.openclaw/workspace/skills/vta-memory/scripts/update-reward.sh \
  --type connection --intensity 0.8 --source "meaningful chat with user"
```

### Track anticipation

When you're looking forward to something:
```bash
~/.openclaw/workspace/skills/vta-memory/scripts/update-reward.sh \
  --anticipate "tomorrow's brainstorming session"
```

## State File Format

```json
{
  "version": "1.1",
  "lastUpdated": "2026-02-01T02:45:00Z",
  "currentDrive": 0.64,
  "baseline": {
    "drive": 0.5
  },
  "recentRewards": [
    {
      "type": "accomplishment",
      "intensity": 0.8,
      "expectedReward": 0.5,
      "rpe": 0.3,
      "source": "completed project",
      "timestamp": "2026-02-01T02:45:00Z"
    }
  ],
  "predictionErrors": [
    {
      "type": "accomplishment",
      "actual": 0.8,
      "expected": 0.5,
      "rpe": 0.3,
      "timestamp": "2026-02-01T02:45:00Z"
    }
  ],
  "anticipating": [
    {"item": "next skill implementation", "addedAt": "2026-02-01T02:45:00Z"}
  ],
  "seeking": [
    {"item": "creative challenges", "addedAt": "2026-02-01T02:45:00Z"}
  ],
  "valueEstimates": {
    "coding": 0.8,
    "research": 0.7
  }
}
```

### New Fields (v1.1)

- **`recentRewards[].expectedReward`** — What reward was expected (default: current drive)
- **`recentRewards[].rpe`** — Calculated Reward Prediction Error
- **`predictionErrors[]`** — History of RPE values for trend analysis (last 100)

## Decay Mechanics

Motivation naturally returns to baseline over time:
- **Decay rate:** 15% of distance to baseline per run
- **Recommended schedule:** Every 6-8 hours
- **Effect:** High motivation fades, but slowly

After 24 hours without updates (3 decay cycles), drive of 0.8 would decay to ~0.69.

## Advanced: Value Estimates

Track learned value associations for different contexts:

```bash
./scripts/update-reward.sh --value "coding" 0.8
./scripts/update-reward.sh --value "meetings" 0.3
```

This builds a learned map of what activities are generally rewarding.

## Neuroscience Background

This skill is based on **Reward Prediction Error (RPE)** theory — one of the most validated theories in neuroscience. The Ventral Tegmental Area produces dopamine that encodes:

- **Positive RPE:** Reward > expected (positive surprise) → dopamine burst → stronger reinforcement
- **Zero RPE:** Reward = expected (prediction correct) → no learning needed
- **Negative RPE:** Reward < expected (disappointment) → dopamine dip → weakening

The implementation uses a simplified Temporal Difference (TD) Learning approach.

### How RPE Works

```
RPE = Actual Reward - Expected Reward
```

Examples:
- Expected 0.5, got 0.8 → RPE = +0.3 (positive surprise! 🎉)
- Expected 0.7, got 0.7 → RPE = 0.0 (as expected ✓)
- Expected 0.8, got 0.4 → RPE = -0.4 (disappointment 😔)

**Key insight:** Drive changes based on RPE, not raw reward intensity. This means:
- Predicted rewards don't boost motivation (you knew it was coming)
- Unexpected rewards strongly boost motivation (positive surprise!)
- Disappointing outcomes reduce motivation (negative prediction error)

See [RPE_THEORY.md](./RPE_THEORY.md) for detailed neuroscience background.

## AI Brain Series

| Part | Function | Status |
|------|----------|--------|
| [hippocampus](https://www.clawhub.ai/skills/hippocampus) | Memory formation, decay, reinforcement | ✅ Live |
| [amygdala-memory](https://www.clawhub.ai/skills/amygdala-memory) | Emotional processing | ✅ Live |
| **vta-memory** | Reward and motivation | ✅ Live |
| [basal-ganglia-memory](https://www.clawhub.ai/skills/basal-ganglia-memory) | Habit formation | 🚧 Development |
| [anterior-cingulate-memory](https://www.clawhub.ai/skills/anterior-cingulate-memory) | Conflict detection | 🚧 Development |
| [insula-memory](https://www.clawhub.ai/skills/insula-memory) | Internal state awareness | 🚧 Development |

## Philosophy

Can an AI *want* things, or only simulate wanting?

Our take: If motivation influences behavior, if the system acts *as if* it wants things... does the distinction matter? Functional wanting might be the only kind that exists for any system — biological or artificial.

---

*Built with ❤️ by the OpenClaw community*
