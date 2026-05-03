# Claude Code セットアップ（共通）

## Claude Code CLI のインストール

### macOS

Homebrew でインストール（推奨）：
```bash
brew install --cask claude-code
```

または、[claude.ai/code](https://claude.ai/code) から直接ダウンロード。

### Debian/Ubuntu

まず `curl` をインストール：
```bash
sudo apt update && sudo apt install curl
```

その後、Claude Code をインストール：
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

または、[claude.ai/code](https://claude.ai/code) から直接ダウンロード。

詳細は [Claude Code 公式ドキュメント](https://code.claude.com/docs/en/quickstart) を参照。

## 認証（ログイン）

公式の推奨フローは「`claude` を起動 → `/login` でアカウント認証」。
Claude Pro / Max / Team / Enterprise サブスクリプション、または Claude Console
（API クレジット）アカウントでログインできます。

### 1. 起動 → ログイン

```bash
claude
# 初回起動時にログインが促される
# セッション内からは /login で再認証も可能
```

一度ログインすれば資格情報はマシンに保存されるため、再ログインは不要。

### 2. 代替: 環境変数で API キーを使う（Console ユーザー向け）

[https://console.anthropic.com/](https://console.anthropic.com/) で生成した
API キーを環境変数として渡すこともできます：

```bash
export ANTHROPIC_API_KEY="sk-ant-..."
```

永続化する場合は `~/.zshrc` または `~/.bashrc` に追加：

```bash
echo 'export ANTHROPIC_API_KEY="sk-ant-..."' >> ~/.zshrc
source ~/.zshrc
```

### 3. インストール確認

```bash
claude --version
```

## Claude Code での作業開始

### リポジトリを Claude で開く

```bash
cd /path/to/repo
claude
```

または

```bash
claude /path/to/repo
```

### Web インターフェース

[https://claude.ai/code](https://claude.ai/code) でブラウザから使用可能

## よく使うコマンド

```bash
# 現在のディレクトリで対話セッションを起動
claude

# 一発タスクを依頼（対話セッションに入る）
claude "fix the build error"

# ワンショット問い合わせ（出力後に終了、スクリプト向き）
claude -p "explain this function"

# 直近の会話を継続
claude -c

# 過去の会話一覧から再開
claude -r

# ヘルプ表示
claude --help
```

## トラブルシューティング

### API キーが見つからない

```bash
echo $ANTHROPIC_API_KEY
# 出力がない場合は設定が必要
```

### CLI が見つからない

```bash
which claude
# /usr/local/bin/claude (macOS, Homebrew)
# ~/.local/bin/claude (Linux, install.sh)
```

パスが通っていない場合、`.zshrc` または `.bashrc` を確認してください。
