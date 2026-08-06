<div align="center">

<img src="https://github.com/DW-dev-UE/DW-dev-UE/blob/main/profile-avatar.png" width="160" height="160" alt="DONGWOOK" />

# 👋 Hi, I'm DONGWOOK

### 🛠️ Full Stack &nbsp;·&nbsp; 🤖 AI Engineer &nbsp;·&nbsp; 🎮 Unreal Engine 5

✨ Scalable systems · intelligent automation · immersive games  
🚀 I ship things that run in the real world.

<br/>

[![GitHub followers](https://img.shields.io/github/followers/DW-dev-UE?label=Follow&style=social)](https://github.com/DW-dev-UE)
![Profile views](https://komarev.com/ghpvc/?username=DW-dev-UE&style=flat-square&color=0abf8a)

<br/>

🌐 [English](README.md) &nbsp;|&nbsp; 🇰🇷 [한국어](README_ko.md) &nbsp;|&nbsp; 🇯🇵 [日本語](README_ja.md)

</div>

---

## 🧑‍💻 About Me

Full stack and AI developer with a strong focus on **Unreal Engine 5**, **Python** backends, automation, and cloud.

💡 I like building systems that scale, measuring performance, and finishing projects that people can actually use — personal builds, lab work, and freelance.

---

## ⭐ Spotlight

### 🧠 [LLM-from-scratch](https://github.com/DW-dev-UE/LLM-from-scratch) · `2026`

> **Apex-1 · 1,119.5M · GPT-class decoder-only · no external weights · Pretrain · SFT · DPO done**

📦 Tokenizer → pretrain → SFT → DPO / GRPO → inference → human-feedback loop  
📚 Docs 🇰🇷 / 🇺🇸 / 🇯🇵 · [BENCHMARK-v2.en.md](https://github.com/DW-dev-UE/LLM-from-scratch/blob/main/BENCHMARK-v2.en.md) (Apex-1)

| 🏷️ | Detail |
|:--|:--|
| ⚙️ Stack | PyTorch · CUDA · BPE tokenizer |
| 📐 Arch | 24L · d2048 · GQA (16Q / 4KV) · RoPE · SwiGLU · RMSNorm · QK-Norm |
| 🏋️ Train | **51K** steps · **~20B** tokens (EN-only) · ctx train **2048** / max **4096** |
| 📊 Scores | HumanEval **8.5** · HellaSwag **46.9** · ARC **41.6** · PIQA **68.6** · GSM8K **1.9** |
| 🤗 Weights | [Apex-1-DPO](https://huggingface.co/YOON1v/Apex-1-DPO) |
| 📎 Also | ~**327M** multilingual `base` ([BENCHMARK-v1.en.md](https://github.com/DW-dev-UE/LLM-from-scratch/blob/main/BENCHMARK-v1.en.md)) · next: **APEX-2 (7B)** |

> [!TIP]
> Full training curves, ablations, and eval methodology live in the benchmark doc linked below.

🔗 [Repository](https://github.com/DW-dev-UE/LLM-from-scratch) &nbsp;·&nbsp; 📊 [Benchmark v2](https://github.com/DW-dev-UE/LLM-from-scratch/blob/main/BENCHMARK-v2.en.md) &nbsp;·&nbsp; 🤗 [HF](https://huggingface.co/YOON1v/Apex-1-DPO)

---

## 🚀 Featured Projects

|  | Project | Year | What it is |
|:--:|:--------|:----:|:-----------|
| 🏫 | **[Konkuk RV.LAB](projects/konkuk-rvlab/Konkuk-RVLab.md)** | 2023–2024 | Situation-based police VR training (Quest · UE5) |
| ⚡ | **[ShiningPass](projects/shiningpass/ShiningPass.md)** | 2025 | Architecture interior client + web (UE5 · Python · AWS) |
| 🖼️ | **[CUDA · YOLOv26 drawing AI](https://github.com/DW-dev-UE/YOLO-to-DALI-CUDA)** | 2026 | YOLO + full CUDA DALI pipeline for drawings |
| 💼 | **[ABadOmic (freelance)](projects/abadomic/ABadOmic.md)** | 2025 | Day/night escape · UE5.6 C++ (dorm / trust / multi-ending) |
| 🎮 | **[STEAM games](projects/steam-games/Steam.md)** | 2023 | Only go up™ · THE LAST BREATH (Y Games / YUNDONGWOOK) |
| 📦 | **[Kmong freelance](projects/kmong/Kmong.md)** | 2022– | UE4/5 outsourcing · 5.0 (35) · 42 deals |
| 🏡 | **[Planterior-AI](https://github.com/DW-dev-UE/Planterior-AI/blob/main/README.md)** | 2025–2026 | AI model → backend → digital twin |

> [!TIP]
> Open any title below for full architecture notes, stack details, and screenshots.

---

## 📦 PyPI Libraries

Published packages you can install with `pip`.

### 🧩 [ComposeLM](https://github.com/DW-dev-UE/ComposeLM) · `composelm` · `2026`

> **One-line configurable modular Transformer · assemble · train · infer**

PyTorch library for building decoder-only LMs from a single `ModelConfig` — presets (Llama / Mistral / Qwen / …), DDP · FSDP2 training, local generation, optional vLLM export.

> [!NOTE]
> No pretrained weights are shipped with the package.

| 🏷️ | Detail |
|:--|:--|
| 📥 Install | `pip install composelm` |
| 🏷️ Version | **1.0.0** (stable) |
| ⚙️ Stack | Python 3.10+ · PyTorch 2.8+ · SafeTensors |
| 🔑 Keywords | modular LLM · ModelConfig · train / resume · GQA · MoE · RoPE |

🔗 [GitHub](https://github.com/DW-dev-UE/ComposeLM) &nbsp;·&nbsp; 📦 [PyPI](https://pypi.org/project/composelm/) &nbsp;·&nbsp; 📚 [Docs](https://github.com/DW-dev-UE/ComposeLM/tree/main/docs)

---

### ⚡ [schema2code](https://github.com/DW-dev-UE/schema2code) · `schema2code` · `2026`

> **LLMs call tools by writing sandboxed Python — not JSON tool calls**

Turn tool schemas into compact Python signatures. The model writes one program; a restricted sandbox runs it. Measured vs classic JSON tool-calling: **−70% tokens**, **−66% round trips**, lower cost, higher accuracy on complex multi-tool tasks.

| 🏷️ | Detail |
|:--|:--|
| 📥 Install | `pip install schema2code` |
| 🏷️ Version | **0.2.0** |
| ⚙️ Stack | Python 3.10+ · **0 runtime deps** |
| 📊 Result | −70% tokens · −66% API turns · sandbox-guard FP = 0 / 60 |

🔗 [GitHub](https://github.com/DW-dev-UE/schema2code) &nbsp;·&nbsp; 📦 [PyPI](https://pypi.org/project/schema2code/) &nbsp;·&nbsp; 📊 [Benchmarks](https://github.com/DW-dev-UE/schema2code/tree/main/benchmarks/results)

---

## 🧰 Tech Stack

**🎮 Engine**

![Unreal](https://skillicons.dev/icons?i=unreal)

**💻 Languages & frontend**

![C](https://skillicons.dev/icons?i=c)
![C++](https://skillicons.dev/icons?i=cpp)
![Python](https://skillicons.dev/icons?i=python)
![React](https://skillicons.dev/icons?i=react)
![HTML](https://skillicons.dev/icons?i=html)
![CSS](https://skillicons.dev/icons?i=css)

**🤖 AI**

![PyTorch](https://skillicons.dev/icons?i=pytorch)
![TensorFlow](https://skillicons.dev/icons?i=tensorflow)
![CUDA](https://skillicons.dev/icons?i=cuda)

**☁️ Cloud & tools**

![AWS](https://skillicons.dev/icons?i=aws)
![GCP](https://skillicons.dev/icons?i=gcp)
![Azure](https://skillicons.dev/icons?i=azure)
![Docker](https://skillicons.dev/icons?i=docker)
![Git](https://skillicons.dev/icons?i=git)
![GitHub](https://skillicons.dev/icons?i=github)
![VS Code](https://skillicons.dev/icons?i=vscode)

---

## 📬 Contact

| | |
|:--|:--|
| ✉️ Email | powerggesa12@naver.com |
| 🐙 GitHub | [@DW-dev-UE](https://github.com/DW-dev-UE) |

---

<div align="center">

### 🙏 Thanks for stopping by!

🌱 A developer who grows steadily — **DONGWOOK**

</div>
