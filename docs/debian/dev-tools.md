# 開発環境（Debian/Ubuntu）

## build-essential

C/C++ のコンパイルや多くのパッケージのビルドに必要：

```bash
sudo apt update
sudo apt install -y build-essential
```

含まれるもの：`gcc`, `g++`, `make`, `dpkg-dev` など。

---

## Docker

### 1. インストール（公式スクリプト）

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
rm get-docker.sh
```

### 2. sudo なしで docker を実行できるようにする

```bash
sudo usermod -aG docker $USER
```

**反映にはログアウト → 再ログインが必要**。または `newgrp docker` で現在のシェルに即時反映。

### 3. サービスの自動起動を有効化

```bash
sudo systemctl enable --now docker
```

### 4. 動作確認

```bash
docker --version
docker compose version
docker run --rm hello-world
```

### 5. Docker Compose

最新の Docker には Compose v2 がプラグインとして同梱されています。
`docker-compose`（ハイフン版）ではなく `docker compose`（スペース版）を使用：

```bash
docker compose up -d
docker compose down
```

---

## Python（pyenv）

### 依存パッケージ

```bash
sudo apt install -y make build-essential libssl-dev zlib1g-dev \
  libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm \
  libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev \
  libffi-dev liblzma-dev
```

### pyenv のインストール

```bash
curl -fsSL https://pyenv.run | bash
```

`.zshrc` または `.bashrc` に追加：

```bash
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"   # bash の場合
# eval "$(pyenv init - zsh)"  # zsh の場合
```

### バージョンインストール例

```bash
pyenv install 3.12
pyenv global 3.12
python --version
```

---

## Node.js（nvm）

### nvm のインストール

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```

新しいシェルで反映：

```bash
exec $SHELL
```

### Node.js のインストール

```bash
nvm install --lts
nvm use --lts
node --version
npm --version
```
