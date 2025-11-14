# Research Environment Templates

このフォルダでは、ホログラフィ・光学・流体・数値計算・機械学習など  
**研究向けの開発環境を Docker で再現するためのテンプレート** をまとめています。

## 📦 Version List

| Version | 用途 | 主な特徴 |
|--------|------|-----------|
| **v1.0.0** | 初期 stable 版 | GPU/CPU 両対応、Julia + Python + C/C++ の統合環境 |

※ 新しいバージョンは互換性や依存環境が変わる場合があるため、  
**必要な環境に応じてフォルダを選んでください。**

---

## 🧭 このディレクトリの目的

研究分野に共通する  
- **再現可能な計算環境（Reproducibility）**  
- **GPU/CPU の切り替え可能性**  
- **CI/CD や実験管理の拡張性**  
- **学生・共同研究者との配布しやすさ**

を達成するためのテンプレート集です。

---

## 📁 Directory Structure

```
research/
├── README.md        ← このファイル（研究環境全体の説明）
└── v1.0.0/          ← バージョン固有のテンプレート
    ├── README.md
    ├── docker/
    ├── docker-compose.yml
    └── ...
```

---

## 🧩 Versioning Policy

バージョンは **Semantic Versioning (MAJOR.MINOR.PATCH)** を採用しています。

- **MAJOR**: 互換性を壊す大規模変更  
- **MINOR**: 新機能追加、構成の拡張  
- **PATCH**: 軽微な修正・依存更新（フォルダは新規作成しない）

---

## 🚀 使い方（共通ワークフロー）

1. 使いたいバージョン（例：`v1.0.0`）に移動  
2. README を読み、`.env` を作成  
3. GPU または CPU で compose を起動  
4. コンテナ内で研究コードを実行

---

## 💡 新バージョンを追加したい場合

バージョンディレクトリをコピーし、以下を変更：

- Dockerfile / compose の依存更新  
- VERSION 表記の更新  
- 新バージョンの README 作成  

例：
```
cp -r v1.0.0 v1.1.0
```

