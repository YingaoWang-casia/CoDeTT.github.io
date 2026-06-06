# 📊 CoDeTT Benchmark

<div align="center">

**CoDeTT: A Context-Aware Decision Benchmark for Turn-Taking Evaluation**

面向全双工语音交互的上下文感知轮次决策评测基准。

Context-aware turn-taking decision benchmark for full-duplex spoken dialogue systems.

<p>
  <a href="https://yingaowang-casia.github.io/CoDeTT.github.io/">
    <img src="https://img.shields.io/badge/Project-Page-111827?style=for-the-badge" alt="Project Page" />
  </a>
  <a href="https://arxiv.org/abs/2603.25434">
    <img src="https://img.shields.io/badge/arXiv-2603.25434-b31b1b?style=for-the-badge&logo=arxiv" alt="arXiv" />
  </a>
  <a href="https://huggingface.co/datasets/YingaoWang-casia/CoDeTT">
    <img src="https://img.shields.io/badge/HuggingFace-Dataset-ffcc4d?style=for-the-badge&logo=huggingface&logoColor=111827" alt="Hugging Face Dataset" />
  </a>
  <a href="https://www.modelscope.cn/datasets/wyawya/CoDeTT">
    <img src="https://img.shields.io/badge/ModelScope-Dataset-2563eb?style=for-the-badge" alt="ModelScope Dataset" />
  </a>
</p>

<p>
  <img src="https://img.shields.io/badge/Task-Turn--Taking-0891b2?style=flat-square" />
  <img src="https://img.shields.io/badge/Dialog-Full--duplex-14b8a6?style=flat-square" />
  <img src="https://img.shields.io/badge/Language-ZH%20%7C%20EN-2563eb?style=flat-square" />
  <img src="https://img.shields.io/badge/Eval-4--Action%20%2B%2014--Intent-7c3aed?style=flat-square" />
  <img src="https://img.shields.io/badge/License-Apache--2.0-0f766e?style=flat-square" />
</p>

</div>

---

## 🔥 Overview

CoDeTT is a benchmark for evaluating **turn-taking decisions** in spoken dialogue systems. Instead of only asking whether a model detects the end of a user utterance, CoDeTT evaluates whether the system can make the right decision under different conversational contexts:

- Should the system **take over** and respond?
- Should it **keep listening** because the user has not finished?
- Should it **maintain** its current speech because the user only gave a backchannel?
- Should it **stop and listen** because the user is interrupting or dismissing the current response?

CoDeTT is designed for scenarios where voice agents need more than silence detection. It focuses on **context-aware decision making**, including user intent, system state, dialogue history, and interaction timing.

---

## 📌 News

- **2026-04-01**: arXiv v2 released.
- **2026-03-26**: Paper submitted to arXiv.
- Dataset and evaluation toolkit are available through this repository, Hugging Face, and ModelScope.

---

## 📄 Paper, Dataset And Project Page

| Resource | Link |
| --- | --- |
| 📄 Paper | [CoDeTT: A Context-Aware Decision Benchmark for Turn-Taking Evaluation](https://arxiv.org/abs/2603.25434) |
| 📕 Local PDF | [`CoDeTT_benchmark/codett.pdf`](CoDeTT_benchmark/codett.pdf) |
| 🌐 Project Page | [yingaowang-casia.github.io/CoDeTT.github.io](https://yingaowang-casia.github.io/CoDeTT.github.io/) |
| 🤗 Hugging Face Dataset | [YingaoWang-casia/CoDeTT](https://huggingface.co/datasets/YingaoWang-casia/CoDeTT) |
| 🔷 ModelScope Dataset | [wyawya/CoDeTT](https://www.modelscope.cn/datasets/wyawya/CoDeTT) |

---

## 🧠 What CoDeTT Evaluates

Traditional turn-taking evaluation is often reduced to binary endpoint detection: whether a user has finished speaking. CoDeTT reframes the problem as **structured decision making**.

### 4-action decision space

| Action | Meaning | Unified evaluation label in toolkit |
| --- | --- | --- |
| `Takeover` | The system should take the conversational floor and respond. | `complete` |
| `Dismiss` | The system should keep waiting or ignore the current input. | `incomplete` |
| `Maintain` | The system should continue its current response. | `backchannel` |
| `Stop & Listen` | The system should stop speaking and listen to the user. | `dismissal` |

### 14 fine-grained intent scenarios

CoDeTT further decomposes turn-taking behavior into fine-grained scenarios under two system states:

| System state | Action group | Fine-grained intent |
| --- | --- | --- |
| `SystemSpeaking` | `Maintain` | `Backchannel`, `Invalidation`, `SideTalk`, `Distraction` |
| `SystemSpeaking` | `StopAndListen` | `Interruption`, `Dismissal`, `Collaboration` |
| `SystemIdle` | `Takeover` | `Completion`, `Cooperation` |
| `SystemIdle` | `Dismiss` | `Incomplete`, `Invalidation`, `Dismissal`, `Exclusion`, `SideTalk` |

This design makes it possible to diagnose cases where a model gets the coarse action right but misunderstands the underlying intent.

---

## 📊 Evaluation Protocol

CoDeTT supports two levels of evaluation:

### Stage 1: Action-level evaluation

All models are evaluated under a unified 4-action / 4-label space:

```text
complete | incomplete | backchannel | dismissal
```

The toolkit reports overall accuracy, per-class accuracy, and binary-style metrics using `complete` as the positive class.

### Stage 2: Intent-level analysis

For models with stronger semantic reasoning capabilities, the benchmark can be extended to inspect the 14 fine-grained intent categories and analyze semantic misalignment.

Typical metrics include:

- `ACC`: action-level prediction accuracy
- per-class accuracy: `complete`, `incomplete`, `backchannel`, `dismissal`
- precision / recall / F1 for `complete`
- false positive rate and false negative rate
- per-source statistics for selected dismissal/wait cases

---

## 🖼️ Figures

<p align="center">
  <img src="CoDeTT_benchmark/image/introduction.png" alt="CoDeTT introduction" width="85%" />
</p>

<p align="center">
  <img src="CoDeTT_benchmark/image/pipline_new.png" alt="CoDeTT pipeline" width="85%" />
</p>

---

## 📦 Repository Structure

```text
.
├── index.html                              # Project page, not required for running benchmark
├── README.md                               # This file
└── CoDeTT_benchmark/
    ├── benchmark.py                        # Main unified benchmark entry
    ├── benchmark_qwen3.py                  # Qwen3-Omni API benchmark
    ├── benchmark_minicpm.py                # MiniCPM local benchmark
    ├── benchmark_ke_semantic.py            # KE-SemanticVAD benchmark
    ├── codett.pdf                          # Paper PDF
    ├── requirements.txt
    ├── image/
    │   ├── introduction.png
    │   ├── pipline_new.png
    │   ├── Table2.png
    │   └── Table3.png
    └── _Adapters/
        ├── easy_turn_wp.py
        ├── smart_turn_wp.py
        ├── ten_turn_wp.py
        ├── firered_wp.py
        ├── namo_wp.py
        └── base.py
```

---

## 🛠️ Environment Setup

```bash
git clone https://github.com/YingaoWang-casia/CoDeTT.github.io.git
cd CoDeTT.github.io/CoDeTT_benchmark

python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

On Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

Notes:

- Python 3.10+ is recommended.
- Some model-specific scripts require additional local checkpoints or API services.
- GPU memory requirements depend on the model being evaluated.

---

## 📥 Dataset Preparation

Download the dataset from one of the official mirrors:

- Hugging Face: [YingaoWang-casia/CoDeTT](https://huggingface.co/datasets/YingaoWang-casia/CoDeTT)
- ModelScope: [wyawya/CoDeTT](https://www.modelscope.cn/datasets/wyawya/CoDeTT)

The benchmark scripts expect JSONL files grouped by language and scenario. The current scripts contain historical default paths such as:

```text
Benchmark_Datasets/jsonls/ZH/syn/...
Benchmark_Datasets/jsonls/EN/syn/...
Benchmark_Datasets/jsonls/EN/real/...
```

Before running on a new machine, update the default dataset paths in the corresponding script, or pass explicit paths when the script supports CLI overrides.

---

## 🚀 Quick Start

### 1. Unified benchmark

```bash
cd CoDeTT_benchmark
python benchmark.py --out_dir ./outputs --run_name exp1
```

`benchmark.py` evaluates supported turn-taking models through a shared protocol. It uses:

- `DEFAULT_DATASETS_ZH`
- `DEFAULT_DATASETS_EN`
- `build_default_paths()`

Please edit these paths if your dataset or model checkpoints are stored elsewhere.

### 2. Qwen3-Omni benchmark

```bash
cd CoDeTT_benchmark
python benchmark_qwen3.py
```

The script calls an OpenAI-compatible local API endpoint. Useful environment variables:

```bash
export QWEN_API_URL="http://localhost:8000/v1/chat/completions"
export QWEN_TIMEOUT=120
export SEND_MODEL_FIELD=0
```

On Windows PowerShell:

```powershell
$env:QWEN_API_URL="http://localhost:8000/v1/chat/completions"
$env:QWEN_TIMEOUT="120"
$env:SEND_MODEL_FIELD="0"
python benchmark_qwen3.py
```

### 3. MiniCPM benchmark

```bash
cd CoDeTT_benchmark
python benchmark_minicpm.py
```

Before running, set `LOCAL_MODEL_DIR` in `benchmark_minicpm.py` to your local MiniCPM checkpoint directory.

### 4. KE-SemanticVAD benchmark

```bash
cd CoDeTT_benchmark
python benchmark_ke_semantic.py \
  --out_dir ./outputs_ke \
  --history_rounds 0 \
  --datasets_user /path/to/user_1.jsonl /path/to/user_2.jsonl \
  --datasets_agent /path/to/agent_1.jsonl /path/to/agent_2.jsonl
```

`history_rounds` controls how much dialogue context is preserved:

| Value | Meaning |
| --- | --- |
| `0` | current utterance only |
| `1` | current utterance + previous 2 messages |
| `2` | current utterance + previous 4 messages |
| `<0` | full history |

---

## 🧪 Supported Baselines And Adapters

The unified benchmark includes adapters for several turn-taking models:

| Adapter | Input / paradigm | Output space |
| --- | --- | --- |
| `EasyTurnWP` | audio | `complete`, `incomplete`, `backchannel`, `dismissal` |
| `SmartTurnWP` | audio / ONNX | binary `complete`, `incomplete` |
| `TENTurnWP` | text / turn detection | `complete`, `incomplete`, `backchannel`, `dismissal` |
| `FireRedChatWP` | text / ONNX | binary `complete`, `incomplete` |
| `NAMOTurnWP` | text / ONNX | binary `complete`, `incomplete` |
| `KESemanticVAD` | semantic VAD | 4-label evaluation |
| `Qwen3-Omni` | multimodal API | 4-label evaluation |
| `MiniCPM` | local multimodal model | 4-label evaluation |

Binary models are evaluated on the supported subset or through compatible mapping logic in the toolkit.

---

## 📁 Outputs

Output directories vary by script, but typically include:

```text
config.json
results.json
report.md
per_sample*.jsonl
error_samples.jsonl
```

Common output meanings:

| File | Meaning |
| --- | --- |
| `config.json` | run configuration, dataset paths, model paths |
| `results.json` | aggregate metrics in JSON |
| `report.md` | human-readable Markdown report |
| `per_sample*.jsonl` | per-sample prediction logs |
| `error_samples.jsonl` | failed or malformed samples, if generated |

---

## ⚠️ Common Notes

1. Some default paths are historical local paths and need to be changed on a new machine.
2. If you see `No dataset files found`, check `DEFAULT_DATASETS_*` or pass dataset paths explicitly.
3. API-based scripts require the local model service to be running before evaluation.
4. Local multimodal model scripts may need large GPU memory; reduce concurrency if OOM occurs.
5. `dismissal` in this toolkit also covers wait / stop-and-listen style cases after normalization.

---

## 📚 Citation

If you use CoDeTT or reference its benchmark design, please cite:

```bibtex
@misc{shen2026codettcontextawaredecisionbenchmark,
  title={CoDeTT: A Context-Aware Decision Benchmark for Turn-Taking Evaluation},
  author={Huan Shen and Yingao Wang and Shangkun Huang and Wei Zou and Yunzhang Chen},
  year={2026},
  eprint={2603.25434},
  archivePrefix={arXiv},
  primaryClass={cs.SD},
  url={https://arxiv.org/abs/2603.25434}
}
```

---

## 📬 Contact

For questions about the dataset, benchmark scripts, or paper, please open an issue in this repository or contact the authors through the project page.
