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

### 🧠 [LLM-from-scratch](https://github.com/DW-dev-UE/LLM-from-scratch)

> **外部 LLM の重みを使わずに作った GPT 級 Decoder-only モデル**

📦 トークナイザー → 事前学習 → SFT → DPO / GRPO → チャット推論 → 人のフィードバックループ  
📚 ドキュメント 🇰🇷 / 🇺🇸 / 🇯🇵 · Base V1 ベンチマークレポート付き

| 🏷️ | 内容 |
|:--|:--|
| ⚙️ スタック | PyTorch · CUDA · BPE トークナイザー |
| 📐 モデル | 約 327M `base`（RMSNorm, RoPE, SwiGLU, GQA） |
| 🔄 状態 | 継続アップデート中 |

🔗 [リポジトリ](https://github.com/DW-dev-UE/LLM-from-scratch) &nbsp;·&nbsp; 📊 [ベンチマーク](https://github.com/DW-dev-UE/LLM-from-scratch/blob/main/BENCHMARK-v1.ja.md)

---

## 🚀 主なプロジェクト

|  | プロジェクト | 概要 |
|:--:|:-------------|:-----|
| 🏫 | **[建国大 RV.LAB](https://github.com/DW-dev-UE/DevLOG/blob/main/Konkuk-RVLab.md)** | 大学研究室の実務プロジェクト |
| ⚡ | **[ShiningPass アーキテクチャ](https://github.com/DW-dev-UE/DevLOG/blob/main/ShiningPass-Architecture.md)** | システム性能最適化（約 340% 向上, 2025） |
| 🖼️ | **[CUDA · YOLOv26 図面 AI](https://github.com/DW-dev-UE/DevLOG/blob/main/CUDA-YOLOv26-AI.md)** | 図面向け AI 自動化 |
| 💼 | **[ABadOmic（フリーランス）](https://github.com/DW-dev-UE/DevLOG/blob/main/ABadOmic-Freelance.md)** | クライアント案件 |
| 🎮 | **[STEAM ゲーム公開](https://github.com/DW-dev-UE/DevLOG/blob/main/STEAM-Game-Release.md)** | Steam でゲームをリリース |
| 📦 | **[Kmong フリーランス](https://github.com/DW-dev-UE/DevLOG/blob/main/Kmong-Freelance.md)** | 複数案件の納品 |
| 🏡 | **[Planterior-AI](https://github.com/DW-dev-UE/Planterior-AI/blob/main/README.md)** | AI モデル → バックエンド → デジタルツイン |

👆 タイトルを開くと概要 · 構成 · 技術 · スクリーンショットを確認できます。

---

## 📦 PyPI ライブラリ

`pip` でインストールできる公開パッケージです。

### 🧩 [ComposeLM](https://github.com/DW-dev-UE/ComposeLM) · `composelm`

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

### ⚡ [schema2code](https://github.com/DW-dev-UE/schema2code) · `schema2code`

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
