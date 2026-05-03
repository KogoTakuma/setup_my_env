# apt セットアップ（Debian/Ubuntu）

## リポジトリ更新

```bash
sudo apt update
sudo apt upgrade
```

## 基本パッケージのインストール

```bash
sudo apt install wget curl htop tree jq
```

## 開発用基本パッケージ

```bash
sudo apt install build-essential
sudo apt install git
sudo apt install vim nano
```

## よく使うツール

```bash
# シェル
sudo apt install zsh
sudo apt install tmux

# 開発ツール
sudo apt install git
sudo apt install curl wget
sudo apt install python3 python3-pip
sudo apt install nodejs npm

# ユーティリティ
sudo apt install fzf
sudo apt install ripgrep
sudo apt install bat
```

## Docker インストール

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker $USER
```

## クリーンアップ

```bash
sudo apt autoremove
sudo apt autoclean
```
