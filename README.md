# 📊 CoDeTT Benchmark

CoDeTT Benchmark 用于评测 Turn-Taking（轮次接管）模型在多场景决策任务中的表现，支持统一四分类评测与多模型对比。

## 📄 论文与数据集

- 📄 论文（arXiv）：https://arxiv.org/abs/2603.25434
- 🤗 数据集（Hugging Face）：https://huggingface.co/datasets/YingaoWang-casia/CoDeTT
- 🔷 数据集（ModelScope）：https://www.modelscope.cn/datasets/wyawya/CoDeTT

## 📦 仓库内容

```
.
├── benchmark.py
├── benchmark_qwen3.py
├── benchmark_minicpm.py
├── benchmark_ke_semantic.py
├── four_class.py
└── requirements.txt
```

## 🛠️ 环境准备

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## 🚀 快速开始

### 📊 主评测
```bash
python benchmark.py --out_dir ./outputs --run_name exp1
```

### 🤖 Qwen3-Omni
```bash
python benchmark_qwen3.py
```

### 🧠 MiniCPM
```bash
python benchmark_minicpm.py
```

### 🔍 KE-SemanticVAD
```bash
python benchmark_ke_semantic.py
```

## 📁 输出结果

- results.json
- report.md
- per_sample*.jsonl
- error_samples.jsonl

## ⚠️ 注意事项

1. 默认路径需手动修改
2. 数据集路径需确认
3. API需提前启动

## 📚 Citation

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
