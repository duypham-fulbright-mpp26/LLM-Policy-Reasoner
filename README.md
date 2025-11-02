# LLM-Policy-Reasoner

### Adaptive Large-Language-Model Framework for Policy Evaluation  
**Author:** Pham Binh Phuong Duy  
**Affiliation:** Master of Public Policy (MPP), Fulbright University Vietnam  
**Thesis Title:** *A Dual-Layer LLM Framework for Policymaking: Policy Evaluation and Public Response Simulation with a Case Study on Personal Income Tax In Vietnam*  
**Year:** 2025  

---

## 🧩 Overview

The **LLM-Policy-Reasoner** is a lightweight reasoning artefact developed as part of a Master’s thesis at Fulbright University Vietnam.  
It adapts the **LLM Economist** framework (Karten et al., 2025) for **policy evaluation** rather than optimisation.

Instead of simulating agents and maximising rewards, this artefact performs **structured reasoning** over fiscal policy trade-offs — particularly focusing on Vietnam’s proposed **Personal Income Tax (PIT)** reform.  
The model evaluates each policy design across three normative dimensions:

1. **Efficiency** – ability to raise revenue with minimal distortion  
2. **Equity** – fairness and proportionality in burden distribution  
3. **Simplicity** – administrative and compliance feasibility  

All evaluations are produced by a locally running LLM through **Ollama**, ensuring privacy, transparency, and reproducibility.

---

## ⚙️ Features

- Modular reasoning system using **Qwen 3 (8B)** via Ollama  
- Structured prompt architecture (system, context, criteria, user layers)  
- Multi-run evaluation (default 20 iterations per policy)  
- Automatic logging and reproducibility controls  
- Built-in retry and progress monitoring  
- Outputs both CSV and JSONL for analysis and audit trail  

---

## 🧱 Directory Structure

```
LLM-Policy-Reasoner/
│
├── policy_inputs/
│   └── pit_vn.json                # 7-bracket baseline + two 5-bracket options
│
├── prompts/
│   ├── policy_eval_system.txt     # LLM system role and output format
│   ├── policy_eval_context_vn.txt # Vietnam PIT reform context (2025 draft)
│   ├── policy_eval_criteria.txt   # Definitions of Efficiency–Equity–Simplicity
│   └── policy_eval_user.txt       # Template for dynamic policy JSON input
│
├── examples/
│   └── policy_eval.py             # Main evaluation script
│
├── outputs/
│   ├── sample_result.csv          # Example run result
│   └── sample_audit.jsonl         # Full reasoning log (optional)
│
└── README.md
```

---

## 🧰 Installation

### 1. Clone and create environment
```bash
git clone https://github.com/duypham/LLM-Policy-Reasoner.git
cd LLM-Policy-Reasoner
conda create -n llmecon python=3.11 -y
conda activate llmecon
pip install requests tqdm pandas
```

### 2. Install and run Ollama
Download Ollama from [https://ollama.com/download](https://ollama.com/download).

Then pull and serve the model:
```bash
ollama pull qwen3:8b
ollama serve
```

The API should be accessible at:
```
http://localhost:11434/api/chat
```

---

## 🚀 Running the Model

### Windows (Anaconda Prompt)
```bat
set POLICY_MODEL=qwen3:8b
set POLICY_TEMP=0.3
set POLICY_SEED=42
set POLICY_NRUNS=20
set POLICY_TIMEOUT=600
python examples\policy_eval.py
```

### macOS / Linux
```bash
export POLICY_MODEL=qwen3:8b
export POLICY_TEMP=0.3
export POLICY_SEED=42
export POLICY_NRUNS=20
export POLICY_TIMEOUT=600
python examples/policy_eval.py
```

Progress will be displayed live.  
After completion, results are stored in:

```
outputs/policy_eval/policy_eval_YYYYMMDD_xxxxxx.csv
outputs/policy_eval/policy_eval_YYYYMMDD_xxxxxx.jsonl
```

---

## 📊 Output Files

| File | Description |
|------|--------------|
| `*.csv` | Structured summary with numeric scores (efficiency, equity, simplicity) and short rationales. |
| `*.jsonl` | Full raw outputs for traceability and validation. |
| `*.log` (optional) | Runtime logs showing retries and progress. |

Example CSV columns:

| policy | efficiency | equity | simplicity | r_efficiency | r_equity | r_simplicity | json_ok | iteration |
|--------|-------------|---------|-------------|---------------|-----------|---------------|----------|------------|

---

## 🧠 Model Configuration Rationale

| Parameter | Value | Justification |
|------------|--------|---------------|
| **Model** | Qwen 3 8B Instruct | Strong bilingual reasoning (English–Vietnamese); runs locally without GPU clusters. |
| **Temperature** | 0.3 | Allows mild variation for nuanced reasoning while maintaining JSON consistency. |
| **Seed** | 42 | Ensures reproducibility across runs. |
| **N_RUNS** | 20 | Following Karten et al. (2025), stability is evaluated via multi-run mean/variance. |
| **Timeout** | 600 s | Ensures completion under CPU-only setups. |

---

## 🔍 Verification & Validation Summary

- **Variance threshold:** σ² < 0.5 across 20 runs  
- **JSON validity:** > 95 % structured responses  
- **Ranking consistency:** identical relative order across runs  
- **Expert alignment:** deviation < 1 point per criterion in validation interviews  

---

## 📚 Citation

If you reuse or reference this artefact, please cite:

> Pham Binh Phuong Duy (2025). *LLM-Policy-Reasoner: A Structured LLM Framework for Policy Evaluation in Vietnam*.  
> Master’s Thesis, Fulbright University Vietnam.  
> GitHub: [https://github.com/duypham/LLM-Policy-Reasoner](https://github.com/duypham/LLM-Policy-Reasoner)

---

## 📜 License

This repository is released under the **MIT License** for non-commercial academic use.  
Please acknowledge the author and institution in derived works.

---

## 🧾 Acknowledgements

This project draws conceptual inspiration from:
- **Karten et al. (2025). LLM Economist Framework** – modular reasoning architecture.  
- **Zheng et al. (2022). AI Economist** – multi-agent policy simulation.  
- **Moore (1995). Creating Public Value** – legitimacy in public management.  

All implementation, adaptation, and evaluation design were independently developed by the author.
