---
name: insula-memory
description: "Internal state awareness for AI agents. Energy, cognitive load, confidence, engagement, and focus tracking. Part of the AI Brain series."
metadata:
  openclaw:
    emoji: "🌡️"
    version: "1.0.0"
    author: "ImpKind"
    requires:
      os: ["darwin", "linux"]
    tags: ["memory", "awareness", "ai-brain", "interoception"]
---

# Insula Memory 🌡️

**Internal state awareness for AI agents.** Part of the AI Brain series.

The insula processes interoception — awareness of internal states. This skill gives AI agents:

- **State tracking** — cognitive load, confidence, engagement, energy, focus
- **Self-monitoring** — knowing when you're overwhelmed, uncertain, or energized
- **"Gut feelings"** — intuitive signals derived from internal state patterns
- **Brain integration** — syncs with VTA, Amygdala, Basal Ganglia, and ACC

## Quick Start

```bash
# Initialize state
skills/insula-memory/scripts/init-state.sh

# Check current state
skills/insula-memory/scripts/get-state.sh

# Load for session context
skills/insula-memory/scripts/load-awareness.sh

# Run full diagnostic
skills/insula-memory/scripts/self-check.sh

# Sync with other brain modules
skills/insula-memory/scripts/integrate-brain.sh
```

## Internal State Dimensions

| Dimension | Range | Description |
|-----------|-------|-------------|
| **cognitiveLoad** | 0.0-1.0 | Mental effort required (0=easy, 1=overwhelmed) |
| **confidence** | 0.0-1.0 | Self-assessed accuracy (0=uncertain, 1=certain) |
| **engagement** | 0.0-1.0 | Interest and attention (0=bored, 1=absorbed) |
| **energy** | 0.0-1.0 | Cognitive resources (0=depleted, 1=fresh) |
| **focus** | 0.0-1.0 | Concentration level (0=scattered, 1=locked-in) |

### Gut Feeling States

Derived automatically from dimension patterns:

| State | Meaning |
|-------|---------|
| **proceed** | All systems go, ready to move forward |
| **caution** | Something's off, proceed carefully |
| **pause** | Need to step back before continuing |
| **overwhelmed** | Too much going on, need to simplify |
| **neutral** | No strong intuition either way |

## Script Reference

### init-state.sh
Initialize or reset the Insula state.

```bash
# First-time setup
./scripts/init-state.sh

# Force reinitialize
./scripts/init-state.sh --force
```

### update-state.sh
Update individual dimensions or decay toward baseline.

```bash
# Set dimension directly
./scripts/update-state.sh --dimension cognitiveLoad --value 0.7 --reason "Complex task"

# Adjust with delta
./scripts/update-state.sh --dimension energy --delta -0.1 --reason "Long session"

# Update multiple at once
./scripts/update-state.sh --all '{"cognitiveLoad": 0.5, "confidence": 0.8}'

# Decay all dimensions toward baseline
./scripts/update-state.sh --decay 0.1
```

### get-state.sh
Display current internal state.

```bash
# Full report with bars and descriptions
./scripts/get-state.sh

# Brief one-liner
./scripts/get-state.sh --brief

# JSON output
./scripts/get-state.sh --json
```

### load-awareness.sh
Human-readable output for session context.

```bash
# Prose format (default)
./scripts/load-awareness.sh

# Brief one-liner
./scripts/load-awareness.sh --format brief

# JSON
./scripts/load-awareness.sh --format json
```

### self-check.sh
Run diagnostic on all internal states.

```bash
# Full diagnostic with recommendations
./scripts/self-check.sh

# Verbose mode
./scripts/self-check.sh --verbose

# JSON output
./scripts/self-check.sh --json
```

### set-baseline.sh
Adjust baseline values for decay.

```bash
# Show current baselines
./scripts/set-baseline.sh

# Set single baseline
./scripts/set-baseline.sh --dimension energy --value 0.8

# Set all baselines from current values
./scripts/set-baseline.sh --from-current

# Reset to defaults
./scripts/set-baseline.sh --reset
```

### integrate-brain.sh
Sync with all other brain modules.

```bash
# Run integration
./scripts/integrate-brain.sh

# Verbose mode (shows which modules found)
./scripts/integrate-brain.sh --verbose

# JSON output
./scripts/integrate-brain.sh --json
```

## Brain Integration

The Insula integrates with all other brain modules:

| Module | Integration |
|--------|-------------|
| **VTA** | High drive → higher engagement; Low drive → lower engagement |
| **Amygdala** | Negative valence → lower confidence; High curiosity → higher engagement |
| **Basal Ganglia** | Many automatic habits → lower cognitive load; High fluency → higher confidence |
| **ACC** | High uncertainty → lower confidence, higher load; Caution mode → lower focus |

Run `integrate-brain.sh` periodically to sync state across all modules.

## Session Startup Integration

Add to AGENTS.md under "Every Session":

```markdown
6. **Load internal state:** `skills/insula-memory/scripts/load-awareness.sh`
   - Or run full brain integration: `skills/insula-memory/scripts/integrate-brain.sh`
```

## State File

Location: `memory/insula-state.json`

```json
{
  "version": "1.0",
  "lastUpdated": "ISO-8601",
  "dimensions": {
    "cognitiveLoad": 0.3,
    "confidence": 0.7,
    "engagement": 0.5,
    "energy": 0.7,
    "focus": 0.6
  },
  "baseline": {
    "cognitiveLoad": 0.3,
    "confidence": 0.7,
    "engagement": 0.5,
    "energy": 0.7,
    "focus": 0.6
  },
  "lastSelfCheck": "ISO-8601",
  "alerts": [],
  "history": [],
  "gutFeeling": "neutral"
}
```

## Cron Setup (Optional)

Decay toward baseline every 6 hours:

```bash
openclaw cron add --name insula-decay \
  --cron '0 */6 * * *' \
  --session isolated \
  --agent-turn 'skills/insula-memory/scripts/update-state.sh --decay 0.1'
```

## AI Brain Series

| Part | Function | Status |
|------|----------|--------|
| [hippocampus-memory](../hippocampus-memory/) | Memory formation, decay, reinforcement | ✅ Active |
| [amygdala-memory](../amygdala-memory/) | Emotional processing | ✅ Active |
| [vta-memory](../vta-memory/) | Reward and motivation | ✅ Active |
| [basal-ganglia-memory](../basal-ganglia-memory/) | Habit formation | ✅ Active |
| [anterior-cingulate-memory](../anterior-cingulate-memory/) | Conflict/error detection | ✅ Active |
| **insula-memory** | Internal state awareness | ✅ Active |

---

*Built with ❤️ by the OpenClaw community*
