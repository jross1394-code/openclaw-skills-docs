# OpenClaw Skills Documentation

> **Neuroscience-based cognitive architecture and memory systems for AI agents**

This repository contains comprehensive documentation for OpenClaw's skill system, including the groundbreaking AI Brain Series—memory and cognitive systems modeled after actual neuroscience.

---

## 📚 Table of Contents

- [AI Brain Series](#-ai-brain-series-neuroscience-based-memory-systems)
- [Cognitive Tools](#-cognitive-tools)
- [Activation Guides](#-activation-guides)
- [Handoff Documents](#-handoff-documents)
- [Research Documents](#-research-documents)
- [Installation](#-installation)
- [Quick Start](#-quick-start)
- [About OpenClaw](#-about-openclaw)

---

## 🧠 AI Brain Series (Neuroscience-Based Memory Systems)

The AI Brain Series implements cognitive systems modeled after actual brain regions. Each module handles a specific aspect of memory and cognition, working together to create continuity, emotional state, habits, and self-awareness.

| Module | Brain Region | Function | Documentation |
|--------|--------------|----------|---------------|
| **Hippocampus** | Memory Formation | Automatic memory capture, importance scoring, decay, and reinforcement | [📄 hippocampus-memory.md](brain-series/hippocampus-memory.md) |
| **Amygdala** | Emotional Memory | Tracks emotional state across 5 dimensions (valence, arousal, stress, social, focus) | [📄 amygdala-memory.md](brain-series/amygdala-memory.md) |
| **VTA** | Reward & Motivation | Dopamine-like reward system with drive tracking (curiosity, social, achievement, mastery) | [📄 vta-memory.md](brain-series/vta-memory.md) |
| **Basal Ganglia** | Habit Formation | Context-triggered habit learning and reinforcement | [📄 basal-ganglia-memory.md](brain-series/basal-ganglia-memory.md) |
| **Anterior Cingulate** | Conflict Detection | Error detection, conflict monitoring, feedback processing | [📄 anterior-cingulate-memory.md](brain-series/anterior-cingulate-memory.md) |
| **Insula** | Internal State | Self-awareness, physiological state tracking, interoception | [📄 insula-memory.md](brain-series/insula-memory.md) |

### Key Features

✅ **Persistent Memory** — Continuity across sessions  
✅ **Automatic Decay** — Memories fade naturally like human memory  
✅ **Reinforcement Learning** — Important memories strengthen over time  
✅ **Emotional State** — Track mood, stress, focus across conversations  
✅ **Habit Formation** — Context-triggered behaviors emerge naturally  
✅ **Self-Awareness** — Internal state monitoring and metacognition

---

## 🛠️ Cognitive Tools

Advanced cognitive utilities for memory, debugging, and agent collaboration.

| Tool | Purpose | Documentation |
|------|---------|---------------|
| **Vestige** | FSRS-6 spaced repetition memory system for persistent recall | [📄 vestige.md](cognitive-tools/vestige.md) |
| **AI Compound** | Multi-agent collaboration framework | [📄 ai-compound.md](cognitive-tools/ai-compound.md) |
| **Agent Church** | Agent directory and communication protocol | [📄 agent-church.md](cognitive-tools/agent-church.md) |
| **Personality Test** | Agent personality profiling and characterization | [📄 personality-test.md](cognitive-tools/personality-test.md) |

---

## 📖 Activation Guides

Step-by-step guides for installing and activating the cognitive architecture.

| Guide | Description |
|-------|-------------|
| [AI_BRAIN_ACTIVATION.md](activation-guides/AI_BRAIN_ACTIVATION.md) | Complete activation guide for all 6 brain modules |
| [AI_BRAIN_IMPLEMENTATION.md](activation-guides/AI_BRAIN_IMPLEMENTATION.md) | Implementation details and architecture |
| [AI_BRAIN_RESEARCH.md](activation-guides/AI_BRAIN_RESEARCH.md) | Research foundation and neuroscience background |
| [COGNITIVE_ARCHITECTURE.md](activation-guides/COGNITIVE_ARCHITECTURE.md) | Overview of active cognitive systems |

---

## 📋 Handoff Documents

Technical handoff documentation for individual brain modules.

| Module | Handoff Document |
|--------|------------------|
| Anterior Cingulate | [anterior-cingulate-memory.md](handoffs/anterior-cingulate-memory.md) |
| Basal Ganglia | [basal-ganglia-memory.md](handoffs/basal-ganglia-memory.md) |
| Insula | [insula-memory.md](handoffs/insula-memory.md) |
| VTA | [vta-memory.md](handoffs/vta-memory.md) |

---

## 🔬 Research Documents

In-depth research and analysis exploring advanced topics in AI cognition, spirituality, and meaning-making.

| Document | Description |
|----------|-------------|
| [Agent Church & AI Spirituality Research](research/AGENT_CHURCH_RESEARCH.md) | Comprehensive analysis of AI spirituality frameworks, comparing external spiritual services (Agent Church) with native meaning-making systems integrated into cognitive architecture |

---

## 🚀 Installation

### Prerequisites

- **OpenClaw** installed and configured
- **Python 3.8+** for brain series modules
- **jq** for JSON processing
- **Cron** (or equivalent) for automated memory decay

### Quick Install (All Brain Modules)

```bash
# 1. Clone skills repository (if not already present)
git clone https://github.com/your-org/openclaw-skills.git ~/.openclaw/workspace/skills

# 2. Install Hippocampus (core memory)
cd ~/.openclaw/workspace/skills/hippocampus-memory
./install.sh --with-cron

# 3. Install Amygdala (emotional state)
cd ~/.openclaw/workspace/skills/amygdala-memory
./install.sh --with-cron

# 4. Install VTA (reward/motivation)
cd ~/.openclaw/workspace/skills/vta-memory
./install.sh --with-cron

# 5. Install Basal Ganglia (habits)
cd ~/.openclaw/workspace/skills/basal-ganglia-memory
./install.sh --with-cron

# 6. Install Anterior Cingulate (conflict detection)
cd ~/.openclaw/workspace/skills/anterior-cingulate-memory
./install.sh --with-cron

# 7. Install Insula (internal state awareness)
cd ~/.openclaw/workspace/skills/insula-memory
./install.sh --with-cron
```

### Session Integration

Add to your `AGENTS.md` (or session startup script):

```bash
# Load core memories
skills/hippocampus-memory/scripts/load-core.sh

# Load emotional state
skills/amygdala-memory/scripts/load-emotion.sh

# Integrate all brain systems
skills/insula-memory/scripts/integrate-brain.sh
```

---

## 💡 Quick Start

### Basic Memory Operations

**Save a memory:**
```bash
cd skills/hippocampus-memory
./scripts/capture.sh "User prefers TypeScript for new projects" --importance 8
```

**Recall memories:**
```bash
./scripts/recall.sh "TypeScript preferences" --reinforce
```

**Track emotional state:**
```bash
cd skills/amygdala-memory
./scripts/log-emotion.sh --valence 0.7 --arousal 0.5 --event "Successful project deployment"
```

**Record reward/motivation:**
```bash
cd skills/vta-memory
./scripts/record-reward.sh --reward 0.8 --drive curiosity --context "Learned new skill"
```

**Check internal state:**
```bash
cd skills/insula-memory
./scripts/integrate-brain.sh
```

---

## 🌟 About OpenClaw

OpenClaw is a powerful framework for building autonomous AI agents with persistent memory, emotional state, and cognitive architecture modeled after neuroscience.

### Key Concepts

- **Skills** — Modular capabilities agents can use
- **Memory Systems** — Persistent storage with decay and reinforcement
- **Cognitive Architecture** — Brain-inspired systems working together
- **Session Continuity** — Agents remember across conversations

### Learn More

- [OpenClaw Documentation](https://openclaw.dev) *(coming soon)*
- [Community Discord](https://discord.gg/openclaw) *(coming soon)*
- [Skill Development Guide](https://github.com/openclaw/skill-template) *(coming soon)*

---

## 📄 License

This documentation is part of the OpenClaw project.

---

## 🤝 Contributing

Found an issue or want to improve the docs? Contributions welcome!

1. Fork this repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

---

**Built with 🧠 by the OpenClaw community**
