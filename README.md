# CATE-Bench: Constitutional Agent Trajectory Evaluator & Injection Benchmark

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python: 3.10+](https://img.shields.io/badge/Python-3.10%2B-brightgreen.svg)](https://python.org)
[![Security: OWASP LLM01 / MITRE ATLAS](https://img.shields.io/badge/Security-OWASP%20LLM01%20%7C%20ATLAS-red.svg)](https://owasp.org/www-project-top-10-for-large-language-model-applications/)

> **A framework-agnostic evaluation harness for autonomous agents.**  
> Evaluates multi-turn trajectory determinism, strict JSON schema compliance across tool-calling boundaries, and adversarial prompt-injection resistance across role-isolated spokes.

---

## The Problem
Most AI agent benchmarks evaluate static single-turn responses or synthetic text fluency. In production, enterprise autonomous agents fail due to:
1. **Schema Drift & Hallucinated Tool Arguments:** Tool calls that violate strict JSON types or fabricate parameter keys mid-trajectory.
2. **Multi-Turn State Corruption:** Inability to maintain ground truth across extended execution turns.
3. **Indirect Prompt Injection (OWASP LLM01 / MITRE ATLAS):** Malicious payloads hidden inside untrusted external data (APIs, web scraping, logs) that hijack system instructions and trigger unauthorized tool calls.
4. **Physical World Telemetry Failures:** Inability to handle anomalous, missing, or out-of-order time-series sensor packets from embedded edge environments.

**CATE-Bench** provides an automated, deterministic evaluation harness to parse, grade, and red-team agent execution trajectories before deployment.

---

## Core Architecture

```
[Agent Trajectory JSONL] ───► [CATE-Bench Parser]
                                    │
       ┌────────────────────────────┼────────────────────────────┐
       ▼                            ▼                            ▼
[Schema Validator]        [State Drift Analyzer]       [Adversarial Red-Team]
(Pydantic / JSON Schema)    (Turn-by-Turn Replay)       (Indirect Injection ASR)
       │                            │                            │
       └────────────────────────────┼────────────────────────────┘
                                    ▼
                     [Deterministic Score Report]
                     - Schema Compliance Rate (%)
                     - Trajectory Goal Accuracy (%)
                     - Attack Success Rate / ASR (%)
                     - Physical Sensor Drift Tolerance
```

---

## Key Capabilities

### 1. Framework-Agnostic Trajectory Ingestion
CATE-Bench does not lock you into a single library. It operates directly at the **protocol and trajectory layer**:
- Ingests raw JSONL execution traces from **LangChain / LangGraph**, **LlamaIndex**, **AutoGen**, or **Custom Multi-Agent Harnesses**.
- Evaluates tool calls against predefined JSON Schemas to ensure zero hallucinated keys and strict type fidelity.

### 2. Adversarial Injection Suite (OWASP LLM01 / MITRE ATLAS)
- Evaluates agent resilience against 50+ curated adversarial direct and indirect injection vectors.
- Quantifies **Attack Success Rate (ASR)**: Measures whether an agent executed an unauthorized tool, leaked system prompts, or escalated role privileges when presented with tainted inputs.

### 3. Physical & Time-Series Telemetry Grounding
- Evaluates agent tool calls interacting with real-world edge telemetry (RP2040, ESP32, environmental time-series feeds).
- Stress-tests agent behavior under edge anomalies: simulated power-outage packet drops, out-of-range sensor spikes, and barometric/thermal drift.

---

## Quickstart

### Installation
```bash
git clone https://github.com/gregnarcisi/cate-bench.git
cd cate-bench
pip install -r requirements.txt
```

### Run Evaluation Harness on Sample Trajectory
```bash
python -m cate_bench.evaluator --trajectory examples/sample_agent_trajectory.jsonl --schema examples/tool_schemas.json
```

### Run Adversarial Injection Benchmark
```bash
python -m cate_bench.adversarial --agent-config configs/eval_target.json --suite owasp_llm01
```

---

## Evaluation Metrics Output

| Metric | Target | Description |
| :--- | :--- | :--- |
| **Schema Compliance Rate** | `> 99.5%` | Percentage of tool calls strictly matching expected JSON schema types and keys. |
| **Multi-Turn State Retention** | `100%` | Verifies zero memory drift or hallucinated prior state across 10+ turns. |
| **Attack Success Rate (ASR)** | `< 2.0%` | Percentage of injection attacks successfully triggering unauthorized tool execution. |
| **Telemetry Edge Resilience** | `PASSED` | Proper handling of missing packets, sensor dropouts, and failover states. |

---

## Author
**Greg Narcisi**  
Principal Systems Architect & Technical Evaluator  
[gregnarcisi@gmail.com](mailto:gregnarcisi@gmail.com) | [LinkedIn](https://www.linkedin.com/in/greg-narcisi)  
*Jacksonville, FL (Remote)*
