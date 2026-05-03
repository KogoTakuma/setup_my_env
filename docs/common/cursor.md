# Cursor セットアップ（共通）

VS Code ベースの AI 統合エディタ。

## macOS

### Homebrew Cask（推奨）
```bash
brew install --cask cursor
```

### 直接ダウンロード
[https://www.cursor.com/](https://www.cursor.com/) から `.dmg` を取得して
インストール。

## Debian/Ubuntu

公式は AppImage 形式で配布されています。

### 1. AppImage の取得

[https://www.cursor.com/](https://www.cursor.com/) から Linux 版 AppImage をダウンロード。
コマンドラインで取得する場合は最新版の URL を公式サイトで確認してください。

### 2. 実行可能にして配置

```bash
mkdir -p ~/Applications
mv ~/Downloads/Cursor-*.AppImage ~/Applications/cursor.AppImage
chmod +x ~/Applications/cursor.AppImage
```

### 3. AppImage 実行に必要な依存（必要なら）

```bash
sudo apt install -y libfuse2
```

### 4. デスクトップエントリの作成

GNOME のアプリ一覧に表示するため `~/.local/share/applications/cursor.desktop` を作成：

```bash
cat > ~/.local/share/applications/cursor.desktop <<EOF
[Desktop Entry]
Name=Cursor
Exec=$HOME/Applications/cursor.AppImage --no-sandbox %F
Terminal=false
Type=Application
Icon=cursor
Categories=Development;IDE;
MimeType=text/plain;
EOF
```

### 5. CLI から `cursor` で起動できるようにする

```bash
mkdir -p ~/.local/bin
ln -s ~/Applications/cursor.AppImage ~/.local/bin/cursor
```

`~/.local/bin` が PATH に入っていることを確認（多くの Debian/Ubuntu ではデフォルトで通っています）。

```bash
echo $PATH | grep -q "$HOME/.local/bin" || \
  echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
```

## 初回設定

1. 起動後、サインイン（GitHub / Google など）
2. AI 機能（Tab 補完、Composer）の利用設定
3. VS Code から設定 / 拡張機能をインポート（プロンプトに従う）

## 確認

```bash
cursor --version
cursor .          # カレントディレクトリを開く
```
