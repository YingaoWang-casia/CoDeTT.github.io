# 📊 CoDeTT Benchmark

<div align="center">

**CoDeTT: A Context-Aware Decision Benchmark for Turn-Taking Evaluation**

面向全双工语音交互的上下文感知轮次决策评测基准。

**中文** | [English](README_EN.md)

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

## 🔥 项目简介

CoDeTT 是一个用于评测语音对话系统 **turn-taking 决策能力** 的基准数据集与评测工具。传统 turn-taking 任务常被简化为“用户是否说完”的端点检测，而 CoDeTT 更关注真实全双工语音交互中的上下文决策：

- 用户说完了吗，系统是否应该 **接管并回复**？
- 用户还没说完，系统是否应该 **继续等待**？
- 用户只是给出 backchannel，系统是否应该 **维持当前回复**？
- 用户正在打断或否定系统，系统是否应该 **停止并倾听**？

CoDeTT 将 turn-taking 从静态端点判断扩展为结合用户意图、系统状态、对话历史和交互时机的结构化决策评测。

---

## 📌 更新

- **2026-04-01**：arXiv v2 发布。
- **2026-03-26**：论文提交至 arXiv。
- 数据集与评测代码已开放在 GitHub、Hugging Face 和 ModelScope。

---

## 📄 论文、数据集与项目页

| 资源 | 链接 |
| --- | --- |
| 📄 论文 | [CoDeTT: A Context-Aware Decision Benchmark for Turn-Taking Evaluation](https://arxiv.org/abs/2603.25434) |
| 📕 本地 PDF | [`CoDeTT_benchmark/codett.pdf`](CoDeTT_benchmark/codett.pdf) |
| 🌐 项目页 | [yingaowang-casia.github.io/CoDeTT.github.io](https://yingaowang-casia.github.io/CoDeTT.github.io/) |
| 🤗 Hugging Face 数据集 | [YingaoWang-casia/CoDeTT](https://huggingface.co/datasets/YingaoWang-casia/CoDeTT) |
| 🔷 ModelScope 数据集 | [wyawya/CoDeTT](https://www.modelscope.cn/datasets/wyawya/CoDeTT) |

---

## 🧠 CoDeTT 评测什么

CoDeTT 将 turn-taking 建模为四类核心动作，并进一步分析 14 个细粒度语义意图场景。

### 四类决策动作

| 动作 | 含义 | 工具中统一标签 |
| --- | --- | --- |
| `Takeover` | 系统应该接管话轮并进行回复。 | `complete` |
| `Dismiss` | 系统应该继续等待，或忽略当前输入。 | `incomplete` |
| `Maintain` | 系统应该继续当前回复。 | `backchannel` |
| `Stop & Listen` | 系统应该停止说话并倾听用户。 | `dismissal` |

### 14 个细粒度意图场景

| 系统状态 | 动作组 | 细粒度意图 |
| --- | --- | --- |
| `SystemSpeaking` | `Maintain` | `Backchannel`, `Invalidation`, `SideTalk`, `Distraction` |
| `SystemSpeaking` | `StopAndListen` | `Interruption`, `Dismissal`, `Collaboration` |
| `SystemIdle` | `Takeover` | `Completion`, `Cooperation` |
| `SystemIdle` | `Dismiss` | `Incomplete`, `Invalidation`, `Dismissal`, `Exclusion`, `SideTalk` |

这种设计可以帮助分析模型到底是粗粒度动作判断错误，还是已经选对动作但误解了更细的语义意图。

---

## 📊 评测协议

CoDeTT 支持两层评测：

### 阶段 1：动作级评测

所有模型都会归一到四类标签空间：

```text
complete | incomplete | backchannel | dismissal
```

评测脚本会输出整体准确率、各类别准确率，以及以 `complete` 为正类时的 precision、recall、F1、FPR 和 FNR 等指标。

### 阶段 2：意图级分析

对于具备更强语义理解能力的模型，可以进一步检查 14 个细粒度意图类别，用来分析模型在不同交互语境下的语义对齐情况。

---

## 🖼️ 图示

<p align="center">
  <img src="CoDeTT_benchmark/image/introduction.png" alt="CoDeTT introduction" width="85%" />
</p>

<p align="center">
  <img src="CoDeTT_benchmark/image/pipline_new.png" alt="CoDeTT pipeline" width="85%" />
</p>

---

## 📦 仓库结构

```text
.
├── index.html
├── README.md
├── README_EN.md
└── CoDeTT_benchmark/
    ├── benchmark.py
    ├── benchmark_qwen3.py
    ├── benchmark_minicpm.py
    ├── benchmark_ke_semantic.py
    ├── codett.pdf
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

## 🛠️ 环境配置

```bash
git clone https://github.com/YingaoWang-casia/CoDeTT.github.io.git
cd CoDeTT.github.io/CoDeTT_benchmark

python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

建议使用 Python 3.10 或更高版本。不同模型脚本可能还需要本地 checkpoint、ONNX 文件或提前启动的本地 API 服务。

---

## 📥 数据准备

请从官方镜像下载数据集：

- Hugging Face: [YingaoWang-casia/CoDeTT](https://huggingface.co/datasets/YingaoWang-casia/CoDeTT)
- ModelScope: [wyawya/CoDeTT](https://www.modelscope.cn/datasets/wyawya/CoDeTT)

评测脚本默认读取按语言和场景划分的 JSONL 文件，历史默认路径类似：

```text
Benchmark_Datasets/jsonls/ZH/syn/...
Benchmark_Datasets/jsonls/EN/syn/...
Benchmark_Datasets/jsonls/EN/real/...
```

在新机器上运行前，请根据本地数据位置修改脚本中的默认路径，或在支持命令行参数的脚本中显式传入数据路径。

---

## 🚀 快速开始

### 1. 统一评测入口

```bash
cd CoDeTT_benchmark
python benchmark.py --out_dir ./outputs --run_name exp1
```

`benchmark.py` 会通过统一协议评测不同 turn-taking 模型。运行前请检查 `DEFAULT_DATASETS_ZH`、`DEFAULT_DATASETS_EN` 和 `build_default_paths()` 中的数据与模型路径。

### 2. Qwen3-Omni

```bash
cd CoDeTT_benchmark
python benchmark_qwen3.py
```

该脚本调用 OpenAI-compatible 的本地 API 服务。常用环境变量如下：

```bash
export QWEN_API_URL="http://localhost:8000/v1/chat/completions"
export QWEN_TIMEOUT=120
export SEND_MODEL_FIELD=0
```

Windows PowerShell:

```powershell
$env:QWEN_API_URL="http://localhost:8000/v1/chat/completions"
$env:QWEN_TIMEOUT="120"
$env:SEND_MODEL_FIELD="0"
python benchmark_qwen3.py
```

### 3. MiniCPM

```bash
cd CoDeTT_benchmark
python benchmark_minicpm.py
```

运行前请在 `benchmark_minicpm.py` 中设置 `LOCAL_MODEL_DIR` 为本地 MiniCPM checkpoint 路径。

### 4. KE-SemanticVAD

```bash
cd CoDeTT_benchmark
python benchmark_ke_semantic.py \
  --out_dir ./outputs_ke \
  --history_rounds 0 \
  --datasets_user /path/to/user_1.jsonl /path/to/user_2.jsonl \
  --datasets_agent /path/to/agent_1.jsonl /path/to/agent_2.jsonl
```

`history_rounds` 控制保留多少轮上下文：

| 取值 | 含义 |
| --- | --- |
| `0` | 仅当前话语 |
| `1` | 当前话语 + 前 2 条消息 |
| `2` | 当前话语 + 前 4 条消息 |
| `<0` | 使用完整历史 |

---

## 🧪 支持的基线与适配器

| 适配器 | 输入 / 范式 | 输出空间 |
| --- | --- | --- |
| `EasyTurnWP` | audio | `complete`, `incomplete`, `backchannel`, `dismissal` |
| `SmartTurnWP` | audio / ONNX | binary `complete`, `incomplete` |
| `TENTurnWP` | text / turn detection | `complete`, `incomplete`, `backchannel`, `dismissal` |
| `FireRedChatWP` | text / ONNX | binary `complete`, `incomplete` |
| `NAMOTurnWP` | text / ONNX | binary `complete`, `incomplete` |
| `KESemanticVAD` | semantic VAD | 4-label evaluation |
| `Qwen3-Omni` | multimodal API | 4-label evaluation |
| `MiniCPM` | local multimodal model | 4-label evaluation |

二分类模型会在工具支持的子集上评测，或通过适配器中的映射逻辑转换到统一标签空间。

---

## 📁 输出结果

不同脚本的输出目录略有差异，通常包含：

```text
config.json
results.json
report.md
per_sample*.jsonl
error_samples.jsonl
```

| 文件 | 含义 |
| --- | --- |
| `config.json` | 本次运行配置、数据路径与模型路径 |
| `results.json` | 聚合指标 |
| `report.md` | 方便阅读的 Markdown 报告 |
| `per_sample*.jsonl` | 每条样本的预测记录 |
| `error_samples.jsonl` | 失败或格式异常样本 |

---

## ⚠️ 注意事项

1. 部分默认路径来自历史实验环境，在新机器上需要手动修改。
2. 如果出现 `No dataset files found`，请检查 `DEFAULT_DATASETS_*` 或显式传入数据路径。
3. API 模型脚本需要先启动本地模型服务。
4. 本地多模态模型可能需要较大显存，显存不足时请降低并发或改用更小模型。
5. 工具中的 `dismissal` 也覆盖归一化后的等待、停止并倾听等情况。

---

## 📚 引用

如果你使用 CoDeTT 数据集、评测脚本或相关 benchmark 设计，请引用：

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

## 📬 联系

如有关于数据集、评测脚本或论文的问题，欢迎在本仓库提交 issue，或通过项目页联系作者。
