<div align="center">
  <h1>FARSIX: Deterministic Cognitive Pipeline Orchestrator & Guardrails Engine</h1>
  <p><b>Advanced Agentic Reasoning and Supervised Intelligence Framework</b></p>
  
  <p>
    <a href="#"><img src="https://img.shields.io/badge/Python-3.11+-blue.svg" alt="Python"></a>
    <a href="#"><img src="https://img.shields.io/badge/NVIDIA-NIM%20API-76B900.svg" alt="NVIDIA"></a>
    <a href="#"><img src="https://img.shields.io/badge/NeMo-Guardrails-red.svg" alt="NeMo"></a>
    <a href="#"><img src="https://img.shields.io/badge/Architecture-Pipeline%20DAG-purple.svg" alt="DAG"></a>
  </p>

  <!-- Autoplaying Demo Video -->
  https://github.com/imFARSI/FARSIX-Deterministic-Pipeline-Orchestrator/raw/master/docs/assets/demo.mp4
</div>

---

## 📌 Executive Summary & The Generative AI Bottleneck

In the modern era of Generative AI, building simple API wrappers around Large Language Models (LLMs) is sufficient for chatbots, but **unreliable for mission-critical enterprise workflows**. 

LLMs suffer fundamentally from non-deterministic logic and hallucinations. If an agent or complex automated reasoning pipeline hallucinates data in a critical analytical report or outputs malformed structural parameters, the downstream APIs will fail unpredictably. 

The industry demands a system that bridges the gap between the cognitive reasoning power of modern high-parameter LLMs and the absolute strictness of traditional deterministic software engineering.

---

## 🚀 The Solution: FARSIX Architecture

**FARSIX** (Framework for Agentic Reasoning and Supervised Intelligence) is a production-grade orchestration pipeline designed specifically to solve the non-deterministic nature of complex LLM pipelines. It is designed to be seamlessly integrated into existing multi-agent systems to act as the overarching deterministic safety layer.

It implements a **Dual-Layer Cognitive Architecture**:

### 1. The Reasoning Layer (NVIDIA NIM)
FARSIX delegates heavy cognitive processing to specialized NVIDIA models:
- **Visual Analysis**: Handled by `Nemotron-Nano-VL-8B` to ingest complex diagrams, UI screenshots, and visual data feeds.
- **Logical Deductions**: Handled by `Llama-3.1-70B-Instruct` to formulate operational reports and complex analytics based on the visual findings.

### 2. The Deterministic Guardrails Layer (NVIDIA NeMo)
A zero-latency, programmatic safety net that intercepts all AI outputs *before* they can be executed by downstream enterprise systems or databases.

<div align="center">
  <img src="docs/assets/demo_1.png" width="100%" alt="FARSIX Dynamic SVG Orchestration Graph">
  <br>
  <i>The real-time SVG Orchestration Dashboard tracking the execution pipeline states.</i>
</div>

---

## 🧠 Core Breakthrough: Dual-Model Semantic Auditing

Traditional guardrails rely on simple Python Regex to validate strings, which completely fails to catch complex logical inconsistencies or disguised hallucinations.

FARSIX integrates **NVIDIA NeMo Guardrails** as a true semantic safety net. Instead of relying purely on regex, it deploys a **Dual-Model Auditing Architecture**:

1. The primary heavy reasoning model (`Llama-3.1-70B`) generates the complex operational report.
2. A fast, specialized auditing model (`Llama-3.1-8B`) instantly ingests the 70B output and uses Colang intent-classification to semantically verify it against safety rules.

By structuring the Colang (`.co`) rules as Semantic Dialogue Flows, FARSIX achieves robust cognitive verification. It actually *understands* the output.
- **Auditor Model**: `meta/llama-3.1-8b-instruct`
- **Validation Latency**: ~1.2 seconds (Standard TTFT)
- **Safety Guarantee**: Absolute. The system physically blocks the pipeline and forces an automatic retry checkpoint if predefined safety boundaries are violated.

---

## 🏗️ System Components & Directory Structure

FARSIX operates as a robust Directed Acyclic Graph (DAG) state machine.

```text
farsix/
├── app.py                     # Entry point for the real-time SVG Dashboard
├── backend/
│   ├── nim_router.py          # The central topological dispatcher 
│   ├── mission_engine.py      # Resilient state machine and recovery system
│   ├── skill_library.py       # ChromaDB vector RAG for mission context
│   └── agents/                # Microservices (Vision, Llama, Guardrails)
├── guardrails/
│   ├── safety_rules.co        # NeMo Input Rail definitions
│   └── actions.py             # Deterministic Python Regex validations
├── memory/                    # ChromaDB persistent storage implementations
└── tests/                     # Unit and integration test suites
```

### Key Subsystems
1. **Dynamic State Machine (`mission_engine.py`)**: A highly resilient execution engine. If a downstream agent fails (e.g., API timeout or Guardrail block), FARSIX intelligently checkpoints the data and resumes from the exact point of failure, rather than restarting the entire mission from scratch.
2. **Skill Memory (`chroma_store.py`)**: A localized vector database that allows the orchestrator to dynamically retrieve contextual knowledge and past successful procedures based on the current mission profile.
3. **Event Bus (`event_bus.py`)**: Fully asynchronous Pub/Sub architecture for non-blocking agent communication.

<div align="center">
  <img src="docs/assets/demo_2.png" width="100%" alt="FARSIX System Metrics and Mission Queue">
  <br>
  <i>System telemetry, active token tracking, and real-time execution logs.</i>
</div>

---

## 🧑‍💻 Developer Guide: Writing Semantic Guardrails

FARSIX allows you to write strict semantic rules that dictate exactly what the AI is allowed to output. This is done via **Colang 1.0** in `guardrails/safety_rules.co`.

Because this uses true LLM intent-classification, the rules are semantic, not regex-bound.

```colang
# guardrails/safety_rules.co
define user said logical inconsistency
  "risk score is 0 but immediate action is required"
  "everything is safe but we must evacuate"

define flow block inconsistency
  user said logical inconsistency
  bot logical inconsistency detected
```

The 8B auditing model will map the 70B output against these intents conceptually, catching variations and obfuscations that standard Regex would miss.

---

## 🧑‍💻 Developer Guide: Programmatic Execution

If you wish to bypass the UI Dashboard and use FARSIX purely as a headless backend API, you can dispatch missions programmatically via the `nim_router`:

```python
import asyncio
from backend.nim_router import NIMRouter

async def execute_headless_mission():
    router = NIMRouter()
    
    # The dispatcher handles Vision -> Reasoning -> Guardrails
    result = await router.dispatch_mission(
        mission_type="visual_qa",
        payload={
            "image_path": "factory_diagram.png",
            "instruction": "Verify if the robot arm path intersects the human walkway."
        }
    )
    
    if result["status"] == "SUCCESS":
        print("Guardrails Passed:", result["final_report"])
    else:
        print("Mission Blocked or Failed:", result["error"])

asyncio.run(execute_headless_mission())
```

---

## 💼 Primary Use Cases

FARSIX is engineered for high-stakes, mission-critical environments:
- **Automated Workflow Auditing**: Analyzing visual feeds or documents to detect operational anomalies.
- **Enterprise QA Systems**: Verifying that an AI's planned output tokens do not violate system constraints before execution.
- **Complex Root-Cause Diagnosis**: Ingesting complex telemetry (CSV/Text) and providing verified, hallucination-free diagnostic reports to engineers.

---

## ⚙️ Installation & Quick Start

FARSIX is designed to run locally, connecting securely to the NVIDIA NIM cloud API.

### 1. Prerequisites
- **Python 3.11+** (Strict requirement for Streamlit and async compatibility)
- An active **NVIDIA NIM API Key**

### 2. Setup
Clone the repository and install the required dependencies:
```bash
git clone https://github.com/imFARSI/FARSIX-Deterministic-Pipeline-Orchestrator.git
cd FARSIX-Deterministic-Pipeline-Orchestrator

pip install -r requirements.txt
```

### 3. Environment Configuration
FARSIX relies on `.env` variables to secure credentials. Create a `.env` file in the root directory:
```env
# Your NVIDIA NIM API Key (required for Llama and Nemotron models)
NVIDIA_API_KEY=nvapi-your-key-here
```
*(Note: Because NeMo Guardrails requires an OpenAI key mapping for its compatibility layer, the internal `guardrails_agent.py` automatically binds `OPENAI_API_KEY` to your NVIDIA key. No secondary key is required).*

### 4. Run the Orchestration Dashboard
To launch the real-time SVG dashboard and mission control interface:
```bash
python -m streamlit run app.py
```
*(Windows users can also simply double-click the included `run.bat` script).*

### 5. Running Tests
FARSIX includes a native NeMo integration test suite to verify the deterministic guardrails without needing to launch the full UI.
```bash
python tests/test_guardrails.py
```

---
*Built for the next generation of Enterprise AI systems.*
