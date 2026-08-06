<div align="center">

<img src="https://github.com/DW-dev-UE/DW-dev-UE/blob/main/profile-avatar.png" width="160" height="160" alt="DONGWOOK" />

# 👋 こんにちは、DONGWOOK です

### 🛠️ Full Stack &nbsp;·&nbsp; 🤖 AI Engineer &nbsp;·&nbsp; 🎮 Unreal Engine 5

✨ スケーラブルなシステム · 自動化 · ゲーム  
🚀 現場で動くものを作るのが好きです。

<br/>

[![GitHub followers](https://img.shields.io/github/followers/DW-dev-UE?label=Follow&style=social)](https://github.com/DW-dev-UE)
![Profile views](https://komarev.com/ghpvc/?username=DW-dev-UE&style=flat-square&color=0abf8a)

<br/>

🌐 [English](README.md) &nbsp;|&nbsp; 🇰🇷 [한국어](README_ko.md) &nbsp;|&nbsp; 🇯🇵 [日本語](README_ja.md)

</div>

---

## 🧑‍💻 自己紹介

**Unreal Engine 5**、**Python** バックエンド、自動化、クラウドを強みとする  
フルスタック · AI エンジニアです。

💡 スケールするシステムを作り、性能を測り、  
個人開発 · 研究室 · フリーランスまで **実際に使える成果** として届けます。

---

## ⭐ Spotlight

### 🧠 [LLM-from-scratch](https://github.com/DW-dev-UE/LLM-from-scratch) · `2026`

> **Apex-1 · 1,119.5M · 外部重みなし GPT 級 Decoder-only · Pretrain · SFT · DPO 完了**

📦 トークナイザー → 事前学習 → SFT → DPO / GRPO → 推論 → 人のフィードバックループ  
📚 ドキュメント 🇰🇷 / 🇺🇸 / 🇯🇵 · [BENCHMARK-v2.ja.md](https://github.com/DW-dev-UE/LLM-from-scratch/blob/main/BENCHMARK-v2.ja.md)（Apex-1）

| 🏷️ | 内容 |
|:--|:--|
| ⚙️ スタック | PyTorch · CUDA · BPE トークナイザー |
| 📐 構成 | 24L · d2048 · GQA（16Q / 4KV）· RoPE · SwiGLU · RMSNorm · QK-Norm |
| 🏋️ 学習 | **51K** step · **約 20B** トークン（EN-only）· ctx train **2048** / max **4096** |
| 📊 スコア | HumanEval **8.5** · HellaSwag **46.9** · ARC **41.6** · PIQA **68.6** · GSM8K **1.9** |
| 🤗 重み | [Apex-1-DPO](https://huggingface.co/YOON1v/Apex-1-DPO) |
| 📎 その他 | 約 **327M** 多言語 `base`（[BENCHMARK-v1.ja.md](https://github.com/DW-dev-UE/LLM-from-scratch/blob/main/BENCHMARK-v1.ja.md)）· 次: **APEX-2（7B）** |

🔗 [リポジトリ](https://github.com/DW-dev-UE/LLM-from-scratch) &nbsp;·&nbsp; 📊 [ベンチマーク v2](https://github.com/DW-dev-UE/LLM-from-scratch/blob/main/BENCHMARK-v2.ja.md) &nbsp;·&nbsp; 🤗 [HF](https://huggingface.co/YOON1v/Apex-1-DPO)

---

## 🚀 主なプロジェクト

|  | プロジェクト | 年 | 概要 |
|:--:|:-------------|:--:|:-----|
| 🏫 | **[建国大 RV.LAB](projects/konkuk-rvlab/Konkuk-RVLab_ja.md)** | 2023–2024 | 状況別警察 VR 訓練（Quest · UE5） |
| ⚡ | **[ShiningPass](projects/shiningpass/ShiningPass_ja.md)** | 2025 | 建築インテリア クライアント + Web（UE5 · Python · AWS） |
| 🖼️ | **[CUDA · YOLOv26 図面 AI](https://github.com/DW-dev-UE/YOLO-to-DALI-CUDA)** | 2026 | YOLO + 完全 CUDA DALI パイプラインの図面 AI |
| 💼 | **[ABadOmic（フリーランス）](projects/abadomic/ABadOmic_ja.md)** | 2025 | 昼/夜エスケープ · UE5.6 C++（宿舎 / 信頼度 / マルチ エンディング） |
| 🎮 | **[STEAM ゲーム](projects/steam-games/Steam_ja.md)** | 2023 | Only go up™ · THE LAST BREATH（Y Games / YUNDONGWOOK） |
| 📦 | **[Kmong フリーランス](projects/kmong/Kmong_ja.md)** | 2022– | UE4/5 外注 · 満足 5.0 (35) · 取引 42 件 |
| 🏡 | **[Planterior-AI](https://github.com/DW-dev-UE/Planterior-AI/blob/main/README.md)** | 2025–2026 | AI モデル → バックエンド → デジタルツイン |

👆 タイトルを開くと概要 · 構成 · 技術 · スクリーンショットを確認できます。

---

## 📦 PyPI ライブラリ

`pip` でインストールできる公開パッケージです。

### 🧩 [ComposeLM](https://github.com/DW-dev-UE/ComposeLM) · `composelm` · `2026`

> **一行設定で組み立てるモジュール型 Transformer · 学習 · 推論**

単一の `ModelConfig` で decoder-only LLM を組み立て · 学習 · 再開 · ローカル推論する PyTorch ライブラリです。Llama / Mistral / Qwen などのプリセット、DDP · FSDP2、ローカル生成、任意の vLLM export に対応。事前学習済み重みは同梱しません。

| 🏷️ | 内容 |
|:--|:--|
| 📥 インストール | `pip install composelm` |
| 🏷️ バージョン | **1.0.0**（stable） |
| ⚙️ スタック | Python 3.10+ · PyTorch 2.8+ · SafeTensors |
| 🔑 キーワード | モジュール型 LLM · ModelConfig · train / resume · GQA · MoE · RoPE |

🔗 [GitHub](https://github.com/DW-dev-UE/ComposeLM) &nbsp;·&nbsp; 📦 [PyPI](https://pypi.org/project/composelm/) &nbsp;·&nbsp; 📚 [ドキュメント](https://github.com/DW-dev-UE/ComposeLM/tree/main/docs)

---

### ⚡ [schema2code](https://github.com/DW-dev-UE/schema2code) · `schema2code` · `2026`

> **LLM が JSON tool call ではなくサンドボックス Python でツールを呼ぶ**

ツールスキーマを短い Python シグネチャに変換。モデルが 1 本のプログラムを書き、制限付きサンドボックスで実行します。従来の JSON tool-calling との測定比較: **トークン −70%**、**ラウンドトリップ −66%**、コスト削減 · 複雑なマルチツールタスクの精度向上。

| 🏷️ | 内容 |
|:--|:--|
| 📥 インストール | `pip install schema2code` |
| 🏷️ バージョン | **0.2.0** |
| ⚙️ スタック | Python 3.10+ · **ランタイム依存 0** |
| 📊 結果 | トークン −70% · API ターン −66% · サンドボックス FP = 0 / 60 |

🔗 [GitHub](https://github.com/DW-dev-UE/schema2code) &nbsp;·&nbsp; 📦 [PyPI](https://pypi.org/project/schema2code/) &nbsp;·&nbsp; 📊 [ベンチマーク](https://github.com/DW-dev-UE/schema2code/tree/main/benchmarks/results)

---

## 🧰 技術スタック

**🎮 エンジン**

![Unreal](https://skillicons.dev/icons?i=unreal)

**💻 言語 · フロント**

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

**☁️ クラウド · ツール**

![AWS](https://skillicons.dev/icons?i=aws)
![GCP](https://skillicons.dev/icons?i=gcp)
![Azure](https://skillicons.dev/icons?i=azure)
![Docker](https://skillicons.dev/icons?i=docker)
![Git](https://skillicons.dev/icons?i=git)
![GitHub](https://skillicons.dev/icons?i=github)
![VS Code](https://skillicons.dev/icons?i=vscode)

---

## 📬 連絡先

| | |
|:--|:--|
| ✉️ Email | powerggesa12@naver.com |
| 🐙 GitHub | [@DW-dev-UE](https://github.com/DW-dev-UE) |

---

<div align="center">

### 🙏 ご覧いただきありがとうございます！

🌱 着実に成長する開発者 — **DONGWOOK**

</div>
