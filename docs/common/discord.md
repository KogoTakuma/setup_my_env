# Discord セットアップ（共通）

## macOS

### Homebrew Cask（推奨）
```bash
brew install --cask discord
```

### 直接ダウンロード
[https://discord.com/download](https://discord.com/download) から `.dmg` を取得して
インストール。

## Debian/Ubuntu

### 公式 .deb パッケージ（推奨）

```bash
# 最新版を取得
wget -O /tmp/discord.deb "https://discord.com/api/download?platform=linux&format=deb"

# インストール
sudo apt install -y /tmp/discord.deb

# クリーンアップ
rm /tmp/discord.deb
```

依存関係不足のエラーが出た場合：

```bash
sudo apt --fix-broken install
```

### Flatpak（自動更新を簡単にしたい場合）

```bash
flatpak install -y flathub com.discordapp.Discord
flatpak run com.discordapp.Discord
```

### Snap

```bash
sudo snap install discord
```

## アップデート

### .deb 版
公式の `.deb` は自動更新されないので、起動時に「Update available」が出たら
再度上記の wget + apt install で上書きインストールするか、Discord 内のアップデートに従う。

### Flatpak / Snap 版
それぞれの仕組みで自動更新されます。

## 起動

```bash
discord
```

または GNOME のアプリ一覧から起動。

## 自動起動の設定（任意）

GNOME の場合、`gnome-tweaks` の Startup Applications か、
`~/.config/autostart/discord.desktop` を作成して設定。
