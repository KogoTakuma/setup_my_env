# Git セットアップマニュアル（共通）

## 1. Git のインストール

### macOS
```bash
brew install git
```

### Debian/Ubuntu
```bash
sudo apt update && sudo apt install git
```

## 2. Git の初期設定

```bash
git config --global user.name "KogoTakuma"
git config --global user.email "1358takuma@gmail.com"
git config --global core.editor "vim"
```

## 3. SSH キーの自動生成と一括設定

ホスト名（マシン名）を自動取得し、識別しやすいファイル名で鍵を生成・設定します。[cite: 2]

```bash
# 環境名（ホスト名）を変数に格納
ENV_NAME=$(hostname -s)
KEY_PATH="$HOME/.ssh/id_ed25519_github_${ENV_NAME}"

# SSHキーの生成（パスフレーズなし）
ssh-keygen -t ed25519 -C "1358takuma@gmail.com" -f "$KEY_PATH" -N ""

# SSH config ファイルへの自動登録
cat << EOF >> ~/.ssh/config

Host github.com
  HostName github.com
  User git
  IdentityFile $KEY_PATH
EOF

# 権限の適正化
chmod 600 ~/.ssh/config
```

## 4. GitHub への登録

1. **公開鍵のコピー**
   以下のコマンドの出力をコピーしてください。[cite: 2]
   ```bash
   cat "$HOME/.ssh/id_ed25519_github_$(hostname -s).pub"
   ```
2. **GitHub での設定**
   - GitHub の [SSH and GPG keys](https://github.com/settings/keys) へアクセス[cite: 2]
   - **New SSH key** をクリック[cite: 2]
   - **Title**: `$(hostname -s)` を実行した際のホスト名を入力[cite: 2]
   - **Key**: 先ほどコピーした内容を貼り付け[cite: 2]

## 5. SSH 接続確認
```bash
ssh -T git@github.com
```

## 6. Git 設定確認

```bash
git config --global --list
```
