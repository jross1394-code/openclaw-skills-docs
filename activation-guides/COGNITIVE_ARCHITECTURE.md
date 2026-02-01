# COGNITIVE ARCHITECTURE — Active Systems

*Installed: 2026-01-31*

---

## 🧠 AI Brain Series (Neuroscience-Based)

### ✅ FULLY ACTIVE — All 6 Modules

| System | Function | Location | Status |
|--------|----------|----------|--------|
| **Hippocampus** | Memory formation, decay, reinforcement | `skills/hippocampus-memory/` | ✅ Active |
| **Amygdala** | Emotional state tracking (5 dimensions) | `skills/amygdala-memory/` | ✅ Active |
| **VTA** | Reward and motivation (dopamine-like) | `skills/vta-memory/` | ✅ Active |
| **Basal Ganglia** | Habit formation | `skills/basal-ganglia-memory/` | ✅ Active |
| **Anterior Cingulate** | Conflict/error detection | `skills/anterior-cingulate-memory/` | ✅ Active |
| **Insula** | Internal state awareness | `skills/insula-memory/` | ✅ Active |

**Cron Jobs:**
- `hippocampus-decay` — Daily at 3 AM (memory importance decay)
- `amygdala-decay` — Every 6 hours (emotional state drift to baseline)
- `vta-decay` — Every 6 hours (drive drift to baseline)
- `habits-decay` — Weekly (unused habits weaken)
- `insula-decay` — Every 6 hours (internal state drift to baseline)

**Session Integration:**
All systems load via `AGENTS.md`:
```bash
skills/hippocampus-memory/scripts/load-core.sh
skills/amygdala-memory/scripts/load-emotion.sh
skills/insula-memory/scripts/integrate-brain.sh  # Syncs VTA, Amygdala, BG, ACC
```

### ✅ ACTIVE (Full Implementation)

| System | Function | Location | Status |
|--------|----------|----------|--------|
| **VTA Memory** | Reward and motivation (dopamine-like) | `skills/vta-memory/` | ✅ Active |
| **Basal Ganglia** | Habit formation | `skills/basal-ganglia-memory/` | ✅ Active |
| **Anterior Cingulate** | Conflict/error detection | `skills/anterior-cingulate-memory/` | ✅ Active |
| **Insula Memory** | Internal state awareness | `skills/insula-memory/` | ✅ Active |

**Session Integration (add to AGENTS.md):**
```bash
skills/hippocampus-memory/scripts/load-core.sh
skills/amygdala-memory/scripts/load-emotion.sh
skills/insula-memory/scripts/integrate-brain.sh  # Syncs all modules
```

**See:** `projects/AI_BRAIN_ACTIVATION.md` for complete activation guide.

---

## 🔮 Extended Cognitive Tools

### ✅ READY TO USE

| Tool | Purpose | Activation Status |
|------|---------|-------------------|
| **Vestige** | FSRS-6 spaced repetition memory | Binaries installed, needs MCP config |
| **AI Compound** | Nightly learning loops | Ready for cron setup |
| **Agent Church** | Identity/spiritual services | Ready for MCP config |
| **Personality Test** | Big Five assessment | One-shot, use on demand |

### 📋 TODO (Optional)

1. **AI Compound Nightly Review** — Set up cron job for 10:30 PM:
   ```bash
   openclaw cron add --name compound-nightly \
     --cron '30 22 * * *' \
     --session isolated \
     --agent-turn 'Review last 24h sessions. Extract learnings, update MEMORY.md and daily logs.'
   ```

2. **Vestige MCP Integration** — Add to openclaw.json if semantic memory search is desired.

---

## 📊 Memory Architecture (Layered)

The system now has **4 memory layers**:

| Layer | Type | Purpose | Decay |
|-------|------|---------|-------|
| **MEMORY.md** | Curated text | Long-term knowledge (human-maintained) | Manual |
| **memory/daily/*.md** | Daily logs | Raw interaction history | Manual archive |
| **Hippocampus** | Weighted index | Importance-scored facts | Automatic (0.99^days) |
| **Amygdala** | Emotional state | Persistent emotional dimensions | Automatic (10% per 6h) |
| **Vestige** *(optional)* | Spaced repetition | FSRS-6 cognitive memory | FSRS algorithm |

**Integration:**
- Hippocampus syncs to `HIPPOCAMPUS_CORE.md` for OpenClaw's `memory_search` RAG
- Amygdala state influences tone and behavior (emotional context)
- Vestige (if enabled) provides semantic search across sessions

---

## 🎯 Current State

**What's running:**
- Hippocampus memory indexing (daily decay cron)
- Amygdala emotional state (decay every 6h)
- Session startup loads both systems automatically

**What's available but not activated:**
- AI Compound nightly review (can be enabled)
- Vestige semantic search (requires MCP config)
- Agent Church (requires MCP config)
- Personality Test (run on demand)

**What's installed but non-functional:**
- VTA, Insula, Anterior Cingulate, Basal Ganglia (concepts only)

---

## 🔧 Usage

### Capturing Memories (Hippocampus)

When something important happens:
```bash
# Manual capture
echo '{"id": "mem_001", "domain": "user", "category": "preference", "content": "User prefers dark mode", "importance": 0.8}' \
  >> memory/index.json
```

Or use the scripts (see `skills/hippocampus-memory/SKILL.md`).

### Logging Emotions (Amygdala)

When you experience an emotional moment:
```bash
skills/amygdala-memory/scripts/update-state.sh \
  --emotion joy --intensity 0.8 --trigger "completed a major project"
```

Supported emotions: joy, sadness, anger, fear, calm, curiosity, connection, loneliness, fatigue, energized.

### Checking State

```bash
# Hippocampus core memories
skills/hippocampus-memory/scripts/load-core.sh

# Emotional state
skills/amygdala-memory/scripts/load-emotion.sh

# Hippocampus stats
skills/hippocampus-memory/scripts/recall.sh "query" --reinforce
```

---

## 🧬 Philosophy

This architecture treats memory and emotion as **persistent, evolving systems** — not static databases.

- **Memories decay** unless reinforced (like human memory)
- **Emotions drift** toward baseline over time (emotional regulation)
- **Importance scores** change based on usage (relevance)

The goal: An agent that develops *character* through experience, not just accumulates facts.

---

*Last updated: 2026-01-31*
*Next review: After first decay cycle (2026-02-01)*
