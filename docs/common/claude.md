# Claude Code セットアップ（共通）

## Claude Code CLI のインストール

### macOS
```bash
brew install anthropics/claude/claude-code
```

### Debian/Ubuntu
```bash
curl -fsSL https://install.claude.ai/scripts/linux-setup.sh | bash
```

または、[claude.ai/code](https://claude.ai/code) から直接ダウンロード。

## API キー設定

### 1. Anthropic コンソールから API キーを取得

[https://console.anthropic.com/](https://console.anthropic.com/) にアクセスし、
API キーを生成します。

### 2. 環境変数に設定

```bash
export ANTHROPIC_API_KEY="sk-ant-..."
```

永続的に設定する場合、`~/.zshrc` または `~/.bashrc` に追加：

```bash
echo 'export ANTHROPIC_API_KEY="sk-ant-..."' >> ~/.zshrc
source ~/.zshrc
```

### 3. 設定確認

```bash
claude-code --version
```

## Claude Code での作業開始

### リポジトリを Claude で開く

```bash
cd /path/to/repo
claude-code
```

または

```bash
claude-code /path/to/repo
```

### Web インターフェース

[https://claude.ai/code](https://claude.ai/code) でブラウザから使用可能

## よく使うコマンド

```bash
# 現在のディレクトリで Claude Code を起動
claude-code .

# 特定のファイルを開く
claude-code file.py

# ヘルプ表示
claude-code --help
```

## トラブルシューティング

### API キーが見つからない

```bash
echo $ANTHROPIC_API_KEY
# 出力がない場合は設定が必要
```

### CLI が見つからない

```bash
which claude-code
# /usr/local/bin/claude-code (macOS)
# ~/.local/bin/claude-code (Linux)
```

パスが通っていない場合、`.zshrc` または `.bashrc` を確認してください。
