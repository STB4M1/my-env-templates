# v1.0.0
GPU / CPU の両方で安定して動作する、再現性重視の科学技術計算向け Docker 環境です。
C/C++・Julia・Python を中心とした研究用途（ホログラフィ・光学・流体・ML等）を
同一構成で再構築できます。


## 🔰 Quick Start

```bash
# 1. リポジトリを取得
git clone https://github.com/STB4M1/my-env-templates.git
cd my-env-templates/research/v1.0.0/docker

# 2. .env を作成
cp .env_template .env

# 3. GPU or CPU で起動
# GPU:
sudo docker compose up -d
# CPU:
sudo docker compose -f docker-compose.cpu.yml up -d

# 4. コンテナへ入る
sudo docker exec -it my-research bash
```

---

## 🧩 .env の UID/GID について
ホスト側のユーザ ID と一致させるための設定です。  
権限トラブル（root 権限ファイル生成など）を防ぐために必須です。

---

## 📁 Structure

```
v1.0.0/
├── docker/
│   ├── Dockerfile
│   └── Dockerfile.cpu
├── .env_template
├── .gitignore
├── docker-compose.yml
├── docker-compose.cpu.yml
└── README.md
```

---

## 🐳 Docker イメージ構成

### CUDA対応版
**ベースイメージ:** `nvidia/cuda:13.0.0-devel-ubuntu24.04`

**主な構成内容:**
- C/C++ 開発ツール: `build-essential`, `cmake`, `git`, `curl`, `pkg-config`
- フォント: `ttf-mscorefonts-installer`, `fontconfig`
- Julia 1.11.6
- Python 3.11（micromamba / conda-forge）
- 科学技術系ライブラリ: `numpy`, `pandas`, `matplotlib`, `scipy`, `scikit-learn`
- CUDA 13.0 対応（`nvcc`, `nvidia-smi`利用可能）

### CPU専用版
**ベースイメージ:** `ubuntu:24.04`

**主な構成内容:**
- CUDAを除き、上記と全く同じ環境構成
- GPU不要、どの環境でも動作可能

---

## 🚀 ビルド & 起動（GPU / CPU）
GPU版: コンテナ名は `my-research`

CPU版: コンテナ名は `my-research-cpu`

### Build Image

```bash
cd <project_root>/docker

# GPU
sudo docker build -t my-cuda-lab:13.0 .

# CPU
sudo docker build -t my-cpu-lab:ubuntu24.04 -f Dockerfile.cpu .
```

### Start Container (docker compose)

```bash
# GPU
sudo docker compose up -d

# CPU
sudo docker compose -f docker-compose.cpu.yml up -d
```

### Enter Container

```bash
# GPU container
sudo docker exec -it my-research bash

# CPU container
sudo docker exec -it my-research-cpu bash
```

### Stop / Remove Container

```bash
sudo docker compose down                          # GPU
sudo docker compose -f docker-compose.cpu.yml down   # CPU
```

### CUDA確認（GPU版のみ）
```bash
nvidia-smi        # GPUが正しく認識されているか確認
nvcc --version    # CUDAコンパイラのバージョンを確認
```


## 🧭 Workflow Tips

### 初回セットアップ
```bash
sudo docker compose up -d          # コンテナ起動
sudo docker exec -it my-research bash  # コンテナに入る
```

### クリーン起動
```bash
sudo docker compose down           # コンテナ停止＋削除
sudo docker compose up -d          # 新規起動
sudo docker exec -it my-research bash
```

### 既存コンテナを再利用
```bash
sudo docker stop my-research       # 停止
sudo docker start my-research      # 再開
sudo docker exec -it my-research bash
```

### YAMLファイル更新後
```bash
docker rm -f my-research           # 古いコンテナ削除
sudo docker compose up -d          # 新しい設定で再起動
sudo docker exec -it my-research bash
```

### 状態確認
```bash
sudo docker ps                     # 起動中コンテナ一覧
sudo docker images                 # イメージ一覧
```

---

## 🛠️ 補足：手動 `docker run`（トラブルシュート用）

通常は **docker compose の利用を推奨**しますが、デバッグ用途で isolate したい場合に
手動 `docker run` を使うこともできます。

### GPU 版

```bash
sudo docker run --gpus all -it \
  -v <project_root>:/workspace \
  -w /workspace \
  my-cuda-lab:13.0 bash
```

### CPU 版

```bash
sudo docker run -it \
  -v <project_root>:/workspace \
  -w /workspace \
  my-cpu-lab:ubuntu24.04 bash
```

> ※ compose と違い、`--name` や UID/GID 設定がないため、
> あくまで **検証・トラブルシュート用途** としての使用を推奨します。

