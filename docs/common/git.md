# Git セットアップ（共通）

## Git のインストール

### macOS
```bash
brew install git
```

### Debian/Ubuntu
```bash
sudo apt update
sudo apt install git
```

## Git の初期設定

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global core.editor "vim"
```

## SSH キー生成

```bash
ssh-keygen -t ed25519 -C "your.email@example.com"
```

生成されたキー：
- 秘密キー: `~/.ssh/id_ed25519`
- 公開キー: `~/.ssh/id_ed25519.pub`

## GitHub への登録

1. `~/.ssh/id_ed25519.pub` の内容をコピー
2. GitHub の Settings > SSH and GPG keys > New SSH key
3. 公開キーを貼り付け

## SSH 接続確認

```bash
ssh -T git@github.com
```

成功時：`Hi username! You've successfully authenticated, but GitHub does not provide shell access.`

## Git 設定確認

```bash
git config --global --list
```
