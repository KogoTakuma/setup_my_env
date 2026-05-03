# Tailscale セットアップ（共通）

WireGuard ベースの VPN/メッシュネットワーク。複数マシンを安全に相互接続できます。

## 1. インストール

### macOS

```bash
brew install --cask tailscale
```

App Store 版もありますが、開発機で使う場合は brew cask 版が簡単です。

### Debian/Ubuntu

公式の APT リポジトリを追加：

```bash
curl -fsSL https://pkgs.tailscale.com/stable/debian/$(lsb_release -cs).noarmor.gpg | \
  sudo tee /usr/share/keyrings/tailscale-archive-keyring.gpg >/dev/null

curl -fsSL https://pkgs.tailscale.com/stable/debian/$(lsb_release -cs).tailscale-keyring.list | \
  sudo tee /etc/apt/sources.list.d/tailscale.list

sudo apt update
sudo apt install -y tailscale
```

## 2. ログインと有効化

```bash
sudo tailscale up
```

ブラウザが開くので（または表示される URL を開いて）、Tailscale アカウントでログイン。
GitHub / Google 等で認証可能。

### 起動時に自動接続（Linux）

```bash
sudo systemctl enable --now tailscaled
```

## 3. 確認

```bash
tailscale status      # 接続中のノード一覧
tailscale ip -4       # 自分の Tailscale IPv4 アドレス
```

## 4. よく使う操作

### 一時的に切断
```bash
sudo tailscale down
```

### 再接続
```bash
sudo tailscale up
```

### Magic DNS 経由でアクセス

```bash
ssh user@<machine-name>      # Tailscale 管理画面で設定したホスト名
```

### サブネット経由のアクセス（必要なら）

```bash
sudo tailscale up --advertise-routes=192.168.1.0/24
```

管理画面でルートを承認する必要があります。

## 5. ログアウト

```bash
sudo tailscale logout
```
