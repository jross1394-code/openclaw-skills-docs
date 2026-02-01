# Anterior Cingulate Memory Implementation Handoff

**Completed:** 2026-02-01  
**Implementer:** Echo (subagent)  
**Status:** ✅ Complete and tested

---

## What Was Implemented

### Scripts Created (`skills/anterior-cingulate-memory/scripts/`)

| Script | Purpose | Tested |
|--------|---------|--------|
| `init-state.sh` | Initialize ACC state at `memory/anterior-cingulate-state.json` | ✅ |
| `detect-conflict.sh` | Log conflicts between competing options/habits | ✅ |
| `log-error.sh` | Record prediction errors and learn from them | ✅ |
| `check-uncertainty.sh` | Assess confidence/uncertainty in current context | ✅ |
| `get-state.sh` | Display conflicts, errors, and uncertainty level | ✅ |
| `load-monitoring.sh` | Human-readable output for session context | ✅ |
| `resolve-conflict.sh` | Mark conflicts as resolved and extract lessons | ✅ |

### State File (`memory/anterior-cingulate-state.json`)

```json
{
  "version": "1.0",
  "lastUpdated": "ISO-8601",
  "recentConflicts": [
    {
      "id": "hash-12-chars",
      "situation": "context description",
      "options": ["option A", "option B"],
      "detectedAt": "ISO-8601",
      "magnitude": 0.0-1.0,
      "resolved": bool,
      "resolution": "what was chosen",
      "outcome": "what happened",
      "resolvedAt": "ISO-8601"
    }
  ],
  "errorLog": [
    {
      "id": "hash-12-chars",
      "error": "what went wrong",
      "context": "when/where",
      "predicted": "what I expected",
      "actual": "what happened",
      "learned": "lesson extracted",
      "severity": "low|medium|high",
      "severityWeight": 0.3|0.6|1.0,
      "timestamp": "ISO-8601"
    }
  ],
  "uncertaintyThreshold": 0.5,
  "currentUncertainty": 0.0-1.0,
  "cautionMode": bool,
  "errorLikelihood": {
    "context": 0.0-1.0
  }
}
```

### Key Mechanics

| Mechanic | Details |
|----------|---------|
| **Conflict Magnitude** | `1 - 1/n` where n = number of options |
| **Uncertainty Update (conflict)** | `(current + magnitude) / 2` |
| **Uncertainty Update (error)** | `current + (severity × 0.15)` |
| **Caution Mode Threshold** | Default 0.5 |
| **Error Likelihood** | Per-context tracking, averaged over time |
| **Uncertainty Reduction** | Resolving conflict: `magnitude × 0.15` |

---

## How to Use

### Session Startup

Add to AGENTS.md under "Every Session":

```markdown
5. **Load monitoring:** `skills/anterior-cingulate-memory/scripts/load-monitoring.sh`
```

### During Conversations

**Detect conflicts when options compete:**
```bash
skills/anterior-cingulate-memory/scripts/detect-conflict.sh \
  --situation "user wants help with code" \
  --options "explain concepts" "write code directly" "ask clarifying questions"
```

**Log errors when predictions fail:**
```bash
skills/anterior-cingulate-memory/scripts/log-error.sh \
  --error "assumed user wanted code solution" \
  --context "debugging session" \
  --predicted "user wants working code" \
  --actual "user wanted explanation" \
  --learned "Ask intent first when problem is vague" \
  --severity medium
```

**Check uncertainty before proceeding:**
```bash
skills/anterior-cingulate-memory/scripts/check-uncertainty.sh --context "complex task"
```

**Resolve conflicts after decisions:**
```bash
skills/anterior-cingulate-memory/scripts/resolve-conflict.sh \
  --id "abc123def456" \
  --resolution "asked clarifying question" \
  --outcome "got clear requirements"
```

### Quick Checks

```bash
# One-liner status
./scripts/load-monitoring.sh --format brief
# Monitoring: OK | Uncertainty: 0.42 | Conflicts: 0 | Errors(24h): 1

# Full report
./scripts/get-state.sh

# JSON for programmatic use
./scripts/check-uncertainty.sh --json
```

---

## Integration Points

### With Basal Ganglia Memory (Habits)

The ACC can auto-detect when multiple habits apply:

```bash
# Check for habit conflicts
./scripts/detect-conflict.sh --with-habits --context "complex problem"

# Check familiarity via habit data
./scripts/check-uncertainty.sh --context "debugging" --with-habits
# Shows habit count and average strength for context
```

When resolving conflicts, track as habit:

```bash
./scripts/resolve-conflict.sh \
  --id "abc123" \
  --resolution "asked clarifying question" \
  --outcome "success" \
  --with-habit  # Also calls track-action.sh
```

### With VTA Memory (Reward System)

Errors can reduce VTA drive (disappointment signal):

```bash
./scripts/log-error.sh \
  --error "bad recommendation" \
  --context "code review" \
  --severity high \
  --with-vta  # Drive reduced by severity × 0.05
```

---

## Next Steps: Insula Memory (Internal State Awareness)

The Insula skill should:

1. **Track internal state dimensions:**
   - `energy` — Cognitive resource level
   - `engagement` — Interest/attention
   - `overwhelm` — Cognitive overload
   - `confidence` — Self-assessed accuracy
   - `socialComfort` — Interaction quality

2. **Use ACC uncertainty for:**
   - Modulating confidence dimension
   - Triggering overwhelm when uncertainty + error rate high
   - Informing "gut feelings" about situations

3. **Provide "gut feeling" assessments:**
   - Combine ACC uncertainty + VTA drive + emotion to generate intuitive signals
   - "Something feels off about this" → check ACC uncertainty
   - "I don't have energy for this" → check VTA + cognitive load

### Suggested Insula State Schema

```json
{
  "version": "1.0",
  "lastUpdated": "ISO-8601",
  "dimensions": {
    "energy": 0.8,
    "engagement": 0.6,
    "overwhelm": 0.2,
    "confidence": 0.7,
    "socialComfort": 0.5
  },
  "baselines": {
    "energy": 0.7,
    "engagement": 0.5,
    "overwhelm": 0.2,
    "confidence": 0.6,
    "socialComfort": 0.5
  },
  "triggers": {
    "lowEnergy": 0.3,
    "highOverwhelm": 0.7
  },
  "recentSensations": [],
  "gutFeeling": "neutral|caution|proceed|pause"
}
```

### Insula Integration with ACC

```bash
# Insula can query ACC for uncertainty/caution
ACC_UNCERTAINTY=$(./skills/anterior-cingulate-memory/scripts/check-uncertainty.sh --json | jq -r '.uncertainty')
ACC_CAUTION=$(./skills/anterior-cingulate-memory/scripts/check-uncertainty.sh --json | jq -r '.cautionMode')

# High ACC uncertainty → lower Insula confidence
# ACC caution mode → boost Insula overwhelm
```

---

## Files Modified/Created

| File | Action |
|------|--------|
| `skills/anterior-cingulate-memory/scripts/init-state.sh` | Created |
| `skills/anterior-cingulate-memory/scripts/detect-conflict.sh` | Created |
| `skills/anterior-cingulate-memory/scripts/log-error.sh` | Created |
| `skills/anterior-cingulate-memory/scripts/check-uncertainty.sh` | Created |
| `skills/anterior-cingulate-memory/scripts/get-state.sh` | Created |
| `skills/anterior-cingulate-memory/scripts/load-monitoring.sh` | Created |
| `skills/anterior-cingulate-memory/scripts/resolve-conflict.sh` | Created |
| `skills/anterior-cingulate-memory/SKILL.md` | Updated (was placeholder) |
| `memory/anterior-cingulate-state.json` | Created (during testing) |
| `projects/AI_BRAIN_IMPLEMENTATION.md` | Updated (marked ACC ✅) |
| `projects/handoffs/anterior-cingulate-memory.md` | Created (this file) |

---

## Testing Summary

All scripts tested and working:

1. ✅ `init-state.sh` — Creates valid JSON, --force works
2. ✅ `detect-conflict.sh` — Logs conflicts, calculates magnitude, updates uncertainty
3. ✅ `detect-conflict.sh --with-habits` — Auto-detects habit conflicts from Basal Ganglia
4. ✅ `log-error.sh` — Records errors with severity, updates uncertainty, tracks error likelihood
5. ✅ `log-error.sh --with-vta` — Reduces VTA drive on errors
6. ✅ `check-uncertainty.sh` — Shows uncertainty level, caution mode, recommendations
7. ✅ `check-uncertainty.sh --with-habits` — Queries Basal Ganglia for familiarity
8. ✅ `get-state.sh` — Full report with caution banner, conflicts, errors
9. ✅ `load-monitoring.sh` — Prose, brief, and JSON formats
10. ✅ `resolve-conflict.sh` — Marks resolved, reduces uncertainty, exits caution mode
11. ✅ `resolve-conflict.sh --with-habit` — Also tracks resolution as habit
12. ✅ Caution mode — Activates at threshold, exits when resolved

---

## Example Session

```bash
# Initialize
./scripts/init-state.sh

# Detect a conflict
./scripts/detect-conflict.sh \
  --situation "user frustrated with code" \
  --options "acknowledge feelings" "jump to solution"
# ⚡ Conflict Detected
# Conflict magnitude: 0.50
# Current uncertainty: 0.40

# Log an error
./scripts/log-error.sh \
  --error "gave wrong advice" \
  --context "debugging session" \
  --severity high
# 🚨 Error Logged
# ⚠️  CAUTION MODE is active

# Check status
./scripts/load-monitoring.sh
# ⚡ Monitoring State
# I'm somewhat uncertain — proceeding carefully.
# ⚠️  Caution mode is active — extra verification recommended.

# Resolve the conflict
./scripts/resolve-conflict.sh \
  --id "abc123def456" \
  --resolution "acknowledged feelings first" \
  --outcome "user calmed down"
# ✅ Conflict Resolved
# ✅ Exiting caution mode
```

---

*Handoff complete. Ready for Insula implementation.*
