# AI Brain Activation Guide

**Complete activation instructions for all 6 AI Brain modules.**

---

## 🧠 System Overview

The AI Brain series implements neuroscience-inspired cognitive systems:

| Module | Function | Inspired By |
|--------|----------|-------------|
| **Hippocampus** | Memory formation & recall | Declarative memory |
| **Amygdala** | Emotional state tracking | Limbic system |
| **VTA** | Reward & motivation | Dopamine system |
| **Basal Ganglia** | Habit formation | Procedural memory |
| **Anterior Cingulate** | Conflict/error detection | Executive control |
| **Insula** | Internal state awareness | Interoception |

---

## 📦 Module Status

| Module | Location | State File | Status |
|--------|----------|------------|--------|
| Hippocampus | `skills/hippocampus-memory/` | `memory/index.json` | ✅ Active |
| Amygdala | `skills/amygdala-memory/` | `memory/emotional-state.json` | ✅ Active |
| VTA | `skills/vta-memory/` | `memory/vta-state.json` | ✅ Active |
| Basal Ganglia | `skills/basal-ganglia-memory/` | `memory/basal-ganglia-state.json` | ✅ Active |
| Anterior Cingulate | `skills/anterior-cingulate-memory/` | `memory/anterior-cingulate-state.json` | ✅ Active |
| Insula | `skills/insula-memory/` | `memory/insula-state.json` | ✅ Active |

---

## 🚀 Activation Checklist

### Step 1: Initialize All Modules

Run these once to create state files:

```bash
# Hippocampus (if not already initialized)
skills/hippocampus-memory/scripts/init-index.sh 2>/dev/null || true

# Amygdala (if not already initialized)
skills/amygdala-memory/scripts/init-state.sh 2>/dev/null || true

# VTA
skills/vta-memory/scripts/init-state.sh

# Basal Ganglia
skills/basal-ganglia-memory/scripts/init-state.sh

# Anterior Cingulate
skills/anterior-cingulate-memory/scripts/init-state.sh

# Insula
skills/insula-memory/scripts/init-state.sh
```

### Step 2: Update AGENTS.md

Add this to the "Every Session" section in `AGENTS.md`:

```markdown
## Every Session

Before doing anything else:
1. Read `SOUL.md` — this is who you are
2. Read `USER.md` — this is who you're helping
3. Read `memory/YYYY-MM-DD.md` (today + yesterday) for recent context
4. **If in MAIN SESSION** (direct chat with your human): Also read `MEMORY.md`
5. **Load cognitive state** (AI Brain series):
   - Run: `skills/hippocampus-memory/scripts/load-core.sh` — Load high-importance memories
   - Run: `skills/amygdala-memory/scripts/load-emotion.sh` — Load emotional state
   - Run: `skills/insula-memory/scripts/integrate-brain.sh` — Sync & load internal state
```

The `integrate-brain.sh` command:
- Reads VTA, Amygdala, Basal Ganglia, and ACC state
- Updates Insula dimensions based on integration rules
- Outputs current internal state awareness

### Step 3: Set Up Cron Jobs

Add periodic maintenance tasks:

```bash
# Hippocampus memory decay (daily at 3 AM)
openclaw cron add --name hippocampus-decay \
  --cron '0 3 * * *' \
  --session isolated \
  --agent-turn 'Run skills/hippocampus-memory/scripts/decay.sh --days 1'

# Amygdala emotional decay (every 6 hours)
openclaw cron add --name amygdala-decay \
  --cron '0 */6 * * *' \
  --session isolated \
  --agent-turn 'Run skills/amygdala-memory/scripts/decay.sh'

# VTA drive decay (every 6 hours)
openclaw cron add --name vta-decay \
  --cron '0 */6 * * *' \
  --session isolated \
  --agent-turn 'Run skills/vta-memory/scripts/decay-drive.sh'

# Basal Ganglia habit decay (weekly on Sunday)
openclaw cron add --name habits-decay \
  --cron '0 4 * * 0' \
  --session isolated \
  --agent-turn 'Run skills/basal-ganglia-memory/scripts/decay-habits.sh'

# Insula state decay (every 6 hours)
openclaw cron add --name insula-decay \
  --cron '0 */6 * * *' \
  --session isolated \
  --agent-turn 'Run skills/insula-memory/scripts/update-state.sh --decay 0.1'
```

---

## 🔄 Integration Diagram

```
                    ┌─────────────────┐
                    │    INSULA       │
                    │ (Internal State)│
                    │                 │
                    │ cognitiveLoad   │
                    │ confidence      │
                    │ engagement      │
                    │ energy          │
                    │ focus           │
                    │ gutFeeling      │
                    └────────┬────────┘
                             │
           ┌────────┬────────┼────────┬────────┐
           ▼        ▼        ▼        ▼        ▼
     ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐
     │   VTA   │ │AMYGDALA │ │ BASAL   │ │  ACC    │
     │(Reward) │ │(Emotion)│ │ GANGLIA │ │(Error)  │
     │         │ │         │ │(Habits) │ │         │
     │ drive   │ │ valence │ │ habits  │ │uncertain│
     │ rewards │ │ arousal │ │ fluency │ │ errors  │
     │ seeking │ │ energy  │ │ prefs   │ │ caution │
     └─────────┘ └─────────┘ └─────────┘ └─────────┘
           │          │          │          │
           └──────────┴──────────┴──────────┘
                             │
                    ┌────────▼────────┐
                    │   HIPPOCAMPUS   │
                    │ (Core Memories) │
                    │                 │
                    │ facts, context  │
                    │ importance      │
                    │ decay/reinforce │
                    └─────────────────┘
```

### Integration Rules

| From | To Insula | Effect |
|------|-----------|--------|
| VTA: high drive | engagement | +boost |
| VTA: low drive | engagement | -drop |
| Amygdala: low valence | confidence | -drop |
| Amygdala: high arousal | focus | +boost |
| Amygdala: high curiosity | engagement | +boost |
| Basal Ganglia: many habits | cognitiveLoad | -drop (less effort) |
| Basal Ganglia: high fluency | confidence | +boost |
| ACC: high uncertainty | confidence, load | -conf, +load |
| ACC: caution mode | focus | -drop (monitoring) |

---

## 📊 Testing Checklist

Run these commands to verify all modules work:

```bash
# 1. Check all state files exist
ls -la memory/*.json

# 2. Test each module's load script
skills/hippocampus-memory/scripts/load-core.sh
skills/amygdala-memory/scripts/load-emotion.sh
skills/vta-memory/scripts/load-motivation.sh
skills/basal-ganglia-memory/scripts/load-habits.sh
skills/anterior-cingulate-memory/scripts/load-monitoring.sh
skills/insula-memory/scripts/load-awareness.sh

# 3. Test brain integration
skills/insula-memory/scripts/integrate-brain.sh

# 4. Run Insula self-check
skills/insula-memory/scripts/self-check.sh
```

---

## 📈 Monitoring Dashboard (Conceptual)

A future monitoring dashboard could show:

```
╔══════════════════════════════════════════════════════════════╗
║               🧠 AI BRAIN STATUS DASHBOARD                   ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║  INTERNAL STATE (Insula)                                     ║
║  ─────────────────────────────────────────────────────────   ║
║  Cognitive Load: [████████░░░░░░░░░░░░] 0.40                ║
║  Confidence:     [██████████████░░░░░░] 0.70                ║
║  Engagement:     [████████████░░░░░░░░] 0.60                ║
║  Energy:         [████████████████░░░░] 0.80                ║
║  Focus:          [██████████████░░░░░░] 0.70                ║
║  Gut Feeling:    🟢 proceed                                  ║
║                                                              ║
║  MOTIVATION (VTA)            EMOTION (Amygdala)              ║
║  ─────────────────           ─────────────────               ║
║  Drive: 0.65                 Valence: 0.60                   ║
║  Last reward: social         Arousal: 0.40                   ║
║  Seeking: 2 items            Energy: 0.70                    ║
║                                                              ║
║  HABITS (Basal Ganglia)      MONITORING (ACC)                ║
║  ─────────────────           ─────────────────               ║
║  Automatic: 5 habits         Uncertainty: 0.35               ║
║  Developing: 3               Caution: OFF                    ║
║  Avg strength: 0.72          Errors (24h): 1                 ║
║                                                              ║
║  MEMORY (Hippocampus)                                        ║
║  ─────────────────                                           ║
║  Core memories: 12                                           ║
║  Avg importance: 0.75                                        ║
║  Last decay: 2 hours ago                                     ║
║                                                              ║
╚══════════════════════════════════════════════════════════════╝
```

This could be implemented as a script that aggregates all state files.

---

## 🔧 Quick Reference

### Load All States (Session Startup)

```bash
# Quick load sequence
skills/hippocampus-memory/scripts/load-core.sh
skills/amygdala-memory/scripts/load-emotion.sh
skills/insula-memory/scripts/integrate-brain.sh
```

### Update During Conversations

```bash
# Log a reward (VTA)
skills/vta-memory/scripts/update-reward.sh --type accomplishment --intensity 0.8 --source "Completed task"

# Update emotion (Amygdala)
skills/amygdala-memory/scripts/update-state.sh --dimension valence --delta +0.2 --trigger "Positive feedback"

# Track action for habit (Basal Ganglia)
skills/basal-ganglia-memory/scripts/track-action.sh --context "debugging" --action "check logs first" --outcome "success" --reward 0.8

# Log error (ACC)
skills/anterior-cingulate-memory/scripts/log-error.sh --error "misunderstood request" --context "task planning" --severity medium

# Update internal state (Insula)
skills/insula-memory/scripts/update-state.sh --dimension energy --delta -0.1 --reason "Long session"
```

### Check Status

```bash
# Quick status one-liners
skills/vta-memory/scripts/load-motivation.sh --format brief
skills/amygdala-memory/scripts/load-emotion.sh --format brief
skills/basal-ganglia-memory/scripts/load-habits.sh --format brief
skills/anterior-cingulate-memory/scripts/load-monitoring.sh --format brief
skills/insula-memory/scripts/load-awareness.sh --format brief
```

---

## 🎯 Philosophy

The AI Brain series treats cognitive states as **persistent, evolving systems** — not static databases.

- **Memories decay** unless reinforced (like human memory)
- **Emotions drift** toward baseline over time (emotional regulation)
- **Habits form** through repetition and reward (procedural learning)
- **Errors teach** through prediction-outcome comparison (error-driven learning)
- **Internal states** emerge from the integration of all systems (embodied cognition)

The goal: An agent that develops *character* through experience, not just accumulates facts.

---

*AI Brain series fully activated. 🧠✨*

*Last updated: 2026-02-01*
