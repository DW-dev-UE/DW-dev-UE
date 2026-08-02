<div align="center">

<img src="https://github.com/DW-dev-UE/DW-dev-UE/blob/main/profile-avatar.png" width="160" height="160" alt="동욱" />

# 👋 안녕하세요, 동욱입니다

### 🛠️ Full Stack &nbsp;·&nbsp; 🤖 AI Engineer &nbsp;·&nbsp; 🎮 Unreal Engine 5

✨ 확장 가능한 시스템 · 자동화 · 게임  
🚀 실제로 돌아가는 결과물을 만드는 걸 좋아합니다.

<br/>

[![GitHub followers](https://img.shields.io/github/followers/DW-dev-UE?label=Follow&style=social)](https://github.com/DW-dev-UE)
![Profile views](https://komarev.com/ghpvc/?username=DW-dev-UE&style=flat-square&color=0abf8a)

<br/>

🌐 [English](README.md) &nbsp;|&nbsp; 🇰🇷 [한국어](README_ko.md) &nbsp;|&nbsp; 🇯🇵 [日本語](README_ja.md)

</div>

---

## 🧑‍💻 소개

**Unreal Engine 5**, **Python** 백엔드, 자동화, 클라우드에 강점을 둔  
풀스택 · AI 개발자입니다.

💡 스케일이 되는 시스템을 만들고, 성능을 재고,  
개인 프로젝트 · 연구실 · 프리랜스까지 **사람들이 쓸 수 있는 결과**로 끝내는 일을 합니다.

---

## ⭐ Spotlight

### 🧠 [LLM-from-scratch](https://github.com/DW-dev-UE/LLM-from-scratch)

> **외부 LLM 가중치 없이 만든 GPT급 Decoder-only 모델**

📦 토크나이저 → 사전학습 → SFT → DPO / GRPO → 채팅 추론 → 인간 피드백 루프  
📚 문서 🇰🇷 / 🇺🇸 / 🇯🇵 · Base V1 벤치마크 리포트 포함

| 🏷️ | 내용 |
|:--|:--|
| ⚙️ 스택 | PyTorch · CUDA · BPE 토크나이저 |
| 📐 모델 | 약 327M `base` (RMSNorm, RoPE, SwiGLU, GQA) |
| 🔄 상태 | 지속적으로 업데이트 중 |

🔗 [저장소](https://github.com/DW-dev-UE/LLM-from-scratch) &nbsp;·&nbsp; 📊 [벤치마크](https://github.com/DW-dev-UE/LLM-from-scratch/blob/main/BENCHMARK-v1.md)

---

## 🚀 주요 프로젝트

|  | 프로젝트 | 한 줄 설명 |
|:--:|:---------|:-----------|
| 🏫 | **[건국대 RV.LAB](https://github.com/DW-dev-UE/DevLOG/blob/main/Konkuk-RVLab.md)** | 대학 연구실 실전 프로젝트 |
| ⚡ | **[ShiningPass 아키텍처](https://github.com/DW-dev-UE/DevLOG/blob/main/ShiningPass-Architecture.md)** | 시스템 성능 최적화 (약 340% 향상, 2025) |
| 🖼️ | **[CUDA · YOLOv26 도면 AI](https://github.com/DW-dev-UE/DevLOG/blob/main/CUDA-YOLOv26-AI.md)** | 도면 자동화 AI 기능 |
| 💼 | **[ABadOmic (프리랜스)](https://github.com/DW-dev-UE/DevLOG/blob/main/ABadOmic-Freelance.md)** | 클라이언트 프로젝트 |
| 🎮 | **[STEAM 게임 출시](https://github.com/DW-dev-UE/DevLOG/blob/main/STEAM-Game-Release.md)** | Steam 플랫폼 게임 출시 |
| 📦 | **[크몽 프리랜스](https://github.com/DW-dev-UE/DevLOG/blob/main/Kmong-Freelance.md)** | 다수 프리랜스 납품 |
| 🏡 | **[Planterior-AI](https://github.com/DW-dev-UE/Planterior-AI/blob/main/README.md)** | AI 모델 → 백엔드 → 디지털 트윈 |

👆 제목을 누르면 개요, 아키텍처, 스택, 스크린샷을 볼 수 있습니다.

---

## 📦 PyPI 라이브러리

`pip` 으로 설치할 수 있는 공개 패키지입니다.

### 🧩 [ComposeLM](https://github.com/DW-dev-UE/ComposeLM) · `composelm`

> **한 줄 설정으로 조립하는 모듈형 Transformer · 학습 · 추론**

단일 `ModelConfig` 로 decoder-only LLM을 조립·학습·재개·로컬 추론하는 PyTorch 라이브러리입니다. Llama / Mistral / Qwen 등 프리셋, DDP · FSDP2, 로컬 생성, 선택적 vLLM export를 지원합니다. 사전학습 가중치는 포함하지 않습니다.

| 🏷️ | 내용 |
|:--|:--|
| 📥 설치 | `pip install composelm` |
| 🏷️ 버전 | **1.0.0** (stable) |
| ⚙️ 스택 | Python 3.10+ · PyTorch 2.8+ · SafeTensors |
| 🔑 키워드 | 모듈형 LLM · ModelConfig · train / resume · GQA · MoE · RoPE |

🔗 [GitHub](https://github.com/DW-dev-UE/ComposeLM) &nbsp;·&nbsp; 📦 [PyPI](https://pypi.org/project/composelm/) &nbsp;·&nbsp; 📚 [문서](https://github.com/DW-dev-UE/ComposeLM/tree/main/docs)

---

### ⚡ [schema2code](https://github.com/DW-dev-UE/schema2code) · `schema2code`

> **LLM이 JSON tool call 대신 샌드박스 Python으로 도구를 호출**

도구 스키마를 짧은 Python 시그니처로 바꿉니다. 모델이 프로그램 하나를 쓰면 제한된 샌드박스에서 실행합니다. 기존 JSON tool-calling 대비 측정 결과: **토큰 −70%**, **라운드트립 −66%**, 비용 절감 · 복잡한 멀티툴 작업 정확도 향상.

| 🏷️ | 내용 |
|:--|:--|
| 📥 설치 | `pip install schema2code` |
| 🏷️ 버전 | **0.2.0** |
| ⚙️ 스택 | Python 3.10+ · **런타임 의존성 0** |
| 📊 결과 | 토큰 −70% · API 턴 −66% · 샌드박스 FP = 0 / 60 |

🔗 [GitHub](https://github.com/DW-dev-UE/schema2code) &nbsp;·&nbsp; 📦 [PyPI](https://pypi.org/project/schema2code/) &nbsp;·&nbsp; 📊 [벤치마크](https://github.com/DW-dev-UE/schema2code/tree/main/benchmarks/results)

---

## 🧰 기술 스택

**🎮 엔진**

![Unreal](https://skillicons.dev/icons?i=unreal)

**💻 언어 · 프론트**

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

**☁️ 클라우드 · 도구**

![AWS](https://skillicons.dev/icons?i=aws)
![GCP](https://skillicons.dev/icons?i=gcp)
![Azure](https://skillicons.dev/icons?i=azure)
![Docker](https://skillicons.dev/icons?i=docker)
![Git](https://skillicons.dev/icons?i=git)
![GitHub](https://skillicons.dev/icons?i=github)
![VS Code](https://skillicons.dev/icons?i=vscode)

---

## 📬 연락처

| | |
|:--|:--|
| ✉️ Email | powerggesa12@naver.com |
| 🐙 GitHub | [@DW-dev-UE](https://github.com/DW-dev-UE) |

---

<div align="center">

### 🙏 방문해 주셔서 감사합니다!

🌱 꾸준히 성장하는 개발자 — **동욱**

</div>
