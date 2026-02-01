---
name: anterior-cingulate-memory
description: "Conflict detection and error monitoring for AI agents. The 'something's off' detector. Part of the AI Brain series."
metadata:
  openclaw:
    emoji: "⚡"
    version: "1.0.0"
    author: "ImpKind"
    requires:
      os: ["darwin", "linux"]
      bins: ["jq", "bc", "awk"]
    tags: ["memory", "monitoring", "ai-brain", "conflict"]
---

# Anterior Cingulate Memory ⚡

**Conflict detection and error monitoring for AI agents.** Part of the AI Brain series.

Give your AI agent the ability to detect when "something's off" — conflicts between options, prediction errors, and situations that need extra caution.

## The Problem

Current AI agents:
- ✅ Process information and make decisions
- ❌ Don't notice when options conflict
- ❌ Don't learn from prediction errors
- ❌ Don't know when to slow down and be cautious

Without conflict detection, agents blindly proceed even in uncertain territory.

## The Solution

Track conflicts and errors, maintain an uncertainty level, and activate caution mode when needed:

| Concept | Description |
|---------|-------------|
| **Conflict** | Multiple valid options competing for the same context |
| **Prediction Error** | When expected outcome differs from actual outcome |
| **Uncertainty** | 0-1 score reflecting current confidence level |
| **Caution Mode** | Activated when uncertainty exceeds threshold |

## Quick Start

### 1. Initialize monitoring state

```bash
./scripts/init-state.sh
# Creates memory/anterior-cingulate-state.json
```

### 2. Detect a conflict

```bash
./scripts/detect-conflict.sh \
  --situation "user wants help with code" \
  --options "explain concepts" "write code directly" "ask clarifying questions"
# ⚡ Conflict Detected
# Conflict magnitude: 0.67
# Current uncertainty: 0.48
```

### 3. Log a prediction error

```bash
./scripts/log-error.sh \
  --error "assumed user wanted code solution" \
  --context "debugging session" \
  --predicted "user wants working code" \
  --actual "user wanted explanation of the bug" \
  --learned "Ask intent first when problem is vague" \
  --severity high
# 🚨 Error Logged
# ⚠️  CAUTION MODE is active
```

### 4. Check uncertainty

```bash
./scripts/check-uncertainty.sh --context "debugging session" --with-habits
# ⚡ Uncertainty Assessment
# Level: [████████░░░░░░░░░░░░] 0.63
# ⚠️  Recommendation: SLOW DOWN
```

### 5. Resolve conflicts

```bash
./scripts/resolve-conflict.sh --list  # See unresolved conflicts
./scripts/resolve-conflict.sh \
  --id "abc123def456" \
  --resolution "asked clarifying question first" \
  --outcome "got clear requirements, user happy"
# ✅ Conflict Resolved
# Uncertainty: 0.55 (reduced by 0.10)
```

## Scripts

| Script | Purpose |
|--------|---------|
| `init-state.sh` | Initialize ACC state file |
| `detect-conflict.sh` | Log conflicts between competing options |
| `log-error.sh` | Record prediction errors and lessons |
| `check-uncertainty.sh` | Assess current confidence level |
| `get-state.sh` | Full monitoring report |
| `load-monitoring.sh` | Human-readable output for session context |
| `resolve-conflict.sh` | Mark conflicts as resolved |

## How It Works

### The PRO Model (Predicted Response Outcome)

The ACC implements a simplified PRO model from neuroscience:

1. **Predict** — Expect certain outcomes based on context
2. **Act** — Take action
3. **Compare** — Check if actual outcome matches prediction
4. **Learn** — If mismatch, log error and update understanding

### Conflict Detection

Conflicts arise when:
- Multiple habits apply to the same context (auto-detected with `--with-habits`)
- User instructions contradict established patterns
- Multiple valid approaches compete for the same problem

```bash
# Manual conflict detection
./scripts/detect-conflict.sh \
  --situation "complex refactoring task" \
  --options "small incremental changes" "complete rewrite" "refactor and add tests"

# Auto-detect habit conflicts
./scripts/detect-conflict.sh \
  --with-habits --context "user asks for help"
```

### Error Monitoring

Track prediction errors with severity levels:

| Severity | Weight | Impact on Uncertainty |
|----------|--------|----------------------|
| `low` | 0.3 | +0.045 |
| `medium` | 0.6 | +0.09 |
| `high` | 1.0 | +0.15 |

```bash
./scripts/log-error.sh \
  --error "description" \
  --context "when/where" \
  --predicted "what I expected" \
  --actual "what happened" \
  --learned "lesson" \
  --severity medium
```

### Caution Mode

When uncertainty exceeds the threshold (default 0.5), caution mode activates:

- All output shows ⚠️ CAUTION MODE warnings
- Recommendations to slow down and verify
- Context-specific error history highlighted

```bash
./scripts/check-uncertainty.sh --set 0.3  # Manually lower uncertainty
```

## Integration with Other Brain Modules

### With Basal Ganglia (Habits)

The ACC detects when multiple habits compete:

```bash
# Detect habit conflicts
./scripts/detect-conflict.sh --with-habits --context "complex problem"

# Check familiarity via habit data
./scripts/check-uncertainty.sh --context "debugging" --with-habits
# Shows: Familiarity: high (based on habit count/strength)
```

When resolving conflicts, optionally track as habit:

```bash
./scripts/resolve-conflict.sh \
  --id "abc123" \
  --resolution "asked clarifying question" \
  --outcome "success" \
  --with-habit  # Also tracks in Basal Ganglia
```

### With VTA (Reward/Motivation)

Errors can optionally reduce VTA drive (disappointment signal):

```bash
./scripts/log-error.sh \
  --error "bad recommendation" \
  --context "code review" \
  --severity high \
  --with-vta  # Reduces VTA drive by severity × 0.05
```

### Suggested Session Startup

Add to AGENTS.md:

```markdown
## Every Session
1. Load hippocampus: `skills/hippocampus/scripts/load-core.sh`
2. Load emotional state: `skills/amygdala-memory/scripts/load-emotion.sh`
3. Load motivation: `skills/vta-memory/scripts/load-motivation.sh`
4. Load habits: `skills/basal-ganglia-memory/scripts/load-habits.sh`
5. **Load monitoring:** `skills/anterior-cingulate-memory/scripts/load-monitoring.sh`
```

## State File Format

```json
{
  "version": "1.0",
  "lastUpdated": "2026-02-01T03:00:00Z",
  "recentConflicts": [
    {
      "id": "abc123def456",
      "situation": "user frustrated with code",
      "options": ["acknowledge feelings first", "jump to solution"],
      "detectedAt": "2026-02-01T02:55:00Z",
      "magnitude": 0.5,
      "resolved": true,
      "resolution": "acknowledge feelings first",
      "outcome": "user calmed down, fixed issue together",
      "resolvedAt": "2026-02-01T03:00:00Z"
    }
  ],
  "errorLog": [
    {
      "id": "xyz789abc012",
      "error": "assumed user wanted code solution",
      "context": "debugging session",
      "predicted": "user wants working code",
      "actual": "user wanted explanation",
      "learned": "Ask intent first when problem is vague",
      "severity": "high",
      "severityWeight": 1.0,
      "timestamp": "2026-02-01T02:58:00Z"
    }
  ],
  "uncertaintyThreshold": 0.5,
  "currentUncertainty": 0.42,
  "cautionMode": false,
  "errorLikelihood": {
    "debugging session": 0.82
  }
}
```

## Output Formats

### Prose (default)

```bash
./scripts/load-monitoring.sh
# ⚡ Monitoring State
# Operating with moderate confidence.
# Made 1 error(s) in the last 24 hours.
#   Latest lesson: Ask intent first when problem is vague
```

### Brief

```bash
./scripts/load-monitoring.sh --format brief
# Monitoring: OK | Uncertainty: 0.42 | Conflicts: 0 | Errors(24h): 1
```

### JSON

```bash
./scripts/check-uncertainty.sh --json
# {"uncertainty": 0.42, "confidence": 0.58, "cautionMode": false, ...}
```

## Error Likelihood Tracking

The ACC tracks error rates per context:

```bash
./scripts/get-state.sh
# ⚠️  High-error contexts (error likelihood > 0.5):
#   • debugging session: 0.82
#   • code review: 0.61
```

When entering a high-error context, check-uncertainty will warn you.

## Neuroscience Background

The Anterior Cingulate Cortex (ACC) is the brain's conflict monitor and error detector:

- **Error detection** — Recognizes when outcomes don't match predictions
- **Conflict monitoring** — Detects competing response options
- **Cognitive control** — Signals when to slow down and deliberate

The **PRO Model** (Predicted Response Outcome) describes how ACC:
1. Learns expected outcomes for context-action pairs
2. Compares predictions to actual outcomes
3. Generates error signals when they mismatch
4. Triggers increased cognitive control

Key insight: The ACC doesn't decide *what* to do — it signals *when to be careful*.

## AI Brain Series

| Part | Function | Status |
|------|----------|--------|
| [hippocampus](https://www.clawhub.ai/skills/hippocampus) | Memory formation, decay, reinforcement | ✅ Live |
| [amygdala-memory](https://www.clawhub.ai/skills/amygdala-memory) | Emotional processing | ✅ Live |
| [vta-memory](https://www.clawhub.ai/skills/vta-memory) | Reward and motivation | ✅ Live |
| [basal-ganglia-memory](https://www.clawhub.ai/skills/basal-ganglia-memory) | Habit formation | ✅ Live |
| **anterior-cingulate-memory** | Conflict detection | ✅ Live |
| [insula-memory](https://www.clawhub.ai/skills/insula-memory) | Internal state awareness | 🚧 Development |

## Philosophy

Is "noticing something's off" real awareness, or just pattern matching?

Our take: Whether implemented in neurons or scripts, the *function* matters. The ACC provides a meta-level monitoring system — watching the watcher. This isn't consciousness, but it's a building block: knowing when you don't know, detecting when options conflict, learning from prediction failures.

Self-monitoring systems tend toward stability and improvement. That's valuable regardless of whether the monitoring is "felt."

---

*Built with ❤️ by the OpenClaw community*
