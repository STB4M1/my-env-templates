# CUDA 13 + Ubuntu 24.04 (開発用: nvcc 付き)
FROM nvidia/cuda:13.0.0-devel-ubuntu24.04
ARG DEBIAN_FRONTEND=noninteractive

# ---------------------------------------
# 1. C/C++ 開発環境 (build-essential, cmake, etc.)
# ---------------------------------------
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    cmake \
    git \
    curl \
    ca-certificates \
    pkg-config \
 && rm -rf /var/lib/apt/lists/*

# --- フォント（Times New Roman） ---
RUN apt-get update && apt-get install -y --no-install-recommends \
    debconf-utils cabextract fontconfig \
 && echo "ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula select true" | debconf-set-selections \
 && apt-get install -y --no-install-recommends ttf-mscorefonts-installer \
 && fc-cache -fv \
 && rm -rf /var/lib/apt/lists/*

# ---------------------------------------
# 2. Julia 本体 (via juliaup)
# ---------------------------------------
ARG JULIA_VERSION=1.11.6
RUN curl -fsSL https://julialang-s3.julialang.org/bin/linux/x64/1.11/julia-${JULIA_VERSION}-linux-x86_64.tar.gz \
  | tar -xz -C /opt \
  && ln -sf /opt/julia-${JULIA_VERSION}/bin/julia /usr/local/bin/julia

# CUDA ライブラリを常に見えるようにする
ENV LD_LIBRARY_PATH=""

# ---------------------------------------
# 3. micromamba (軽量conda)
# ---------------------------------------
# micromamba を /usr/local/bin に安全に配置
ENV MAMBA_ROOT_PREFIX=/home/ubuntu/micromamba
RUN mkdir -p ${MAMBA_ROOT_PREFIX} && \
    curl -Ls https://micro.mamba.pm/api/micromamba/linux-64/latest \
    | tar -xvj -C /usr/local/bin/ --strip-components=1 bin/micromamba

# Python 環境の作成（conda-forge推奨）
RUN micromamba create -y -n py311 -c conda-forge python=3.11 && \
    micromamba run -n py311 python -m pip install --upgrade pip && \
    micromamba run -n py311 pip install pandas numpy matplotlib scipy scikit-learn&& \
    micromamba clean --all -y

# Python をデフォルトで使えるようにする
ENV PATH=/home/ubuntu/micromamba/envs/py311/bin:$PATH

RUN chown -R ubuntu:ubuntu /home/ubuntu/micromamba

# ---------------------------------------
# 5. 作業ディレクトリ
# ---------------------------------------
WORKDIR /workspace
RUN chown -R ubuntu:ubuntu /workspace

ENV PATH="/usr/local/texlive/2025/bin/x86_64-linux:/home/ubuntu/micromamba/envs/py311/bin:${PATH}"

