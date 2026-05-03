# セットアップ手順マスターリスト

## 最優先セットアップ

### Git セットアップ
- [ ] Git のインストール
- [ ] SSH キーの生成
- [ ] GitHub の設定・接続確認
- [ ] Git グローバル設定（user.name, user.email）
- [ ] SSH Config の設定

### Claude Code セットアップ
- [ ] Claude Code CLI のインストール
- [ ] API キーの設定（ANTHROPIC_API_KEY）
- [ ] 基本設定の確認
- [ ] このリポジトリの clone
- [ ] Claude Code でリポジトリを開く

---

## 共通セットアップ

### パッケージマネージャー
- [ ] パッケージマネージャーの更新（apt update / brew update）
- [ ] 基本ツールのインストール

### シェル設定
- [ ] zsh のインストール・設定
- [ ] oh-my-zsh のセットアップ
- [ ] プラグインのインストール
- [ ] .bashrc / .zshrc の設定

### 基本ツール
- [ ] curl / wget のインストール
- [ ] vim / nano のセットアップ
- [ ] tmux のインストール
- [ ] fzf のインストール
- [ ] tree / jq のインストール

---

## macOS 固有セットアップ

### Homebrew
- [ ] Homebrew のインストール
- [ ] 基本パッケージのインストール
- [ ] Cask でアプリケーションのインストール

### 開発環境
- [ ] Xcode Command Line Tools のインストール
- [ ] Python のセットアップ（pyenv）
- [ ] Node.js のセットアップ（nvm）
- [ ] Docker のインストール

### macOS 設定
- [ ] キーボード設定
- [ ] Finder の設定
- [ ] Dock のカスタマイズ
- [ ] Spotlight の設定

---

## Debian/Ubuntu 固有セットアップ

### apt パッケージマネージャー
- [ ] apt リポジトリの更新
- [ ] 必須パッケージのインストール
- [ ] snap パッケージの設定

### 開発環境
- [ ] build-essential のインストール
- [ ] Python のセットアップ（pyenv）
- [ ] Node.js のセットアップ（nvm）
- [ ] Docker のセットアップ

### システム設定
- [ ] タイムゾーンの設定
- [ ] ロケール設定
- [ ] サスペンド・ロック設定

---

## 開発ツール共通

### バージョン管理
- [ ] Git プロフィールの設定
- [ ] GPG キーの設定

### 言語・フレームワーク
- [ ] Node.js (nvm) のセットアップ
- [ ] Python (pyenv/venv) のセットアップ
- [ ] Go のセットアップ
- [ ] Rust のセットアップ

### CLI ツール
- [ ] aws-cli のセットアップ
- [ ] gcloud CLI のセットアップ
- [ ] kubectl のセットアップ
- [ ] terraform のセットアップ

---

## 最終確認
- [ ] PATH が正しく設定されているか確認
- [ ] すべてのツールがアクセス可能か確認
- [ ] シェル設定の再読み込み完了
- [ ] git push で動作確認
