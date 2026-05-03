# シェル設定（共通）

zsh + Oh My Zsh + プラグインで快適なシェル環境を構築します。

## 1. zsh のインストール

### macOS
```bash
brew install zsh
```

### Debian/Ubuntu
```bash
sudo apt update && sudo apt install -y zsh
```

### デフォルトシェルに変更

```bash
chsh -s "$(which zsh)"
```

ログアウト → 再ログインで反映。

## 2. Oh My Zsh のインストール

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

インストール時に既存の `~/.zshrc` がバックアップされ、新しい `~/.zshrc` が生成されます。

## 3. プラグインのインストール

### zsh-autosuggestions（コマンド履歴の補完候補表示）

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions \
  ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

### zsh-syntax-highlighting（シンタックスハイライト）

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git \
  ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

### `~/.zshrc` のプラグイン設定

```bash
plugins=(
  git
  zsh-autosuggestions
  zsh-syntax-highlighting
)
```

設定の反映：

```bash
source ~/.zshrc
```

## 4. テーマ（任意）

デフォルトの `robbyrussell` 以外を使う場合：

```bash
# 例: agnoster（Powerline フォントが必要）
ZSH_THEME="agnoster"

# 例: 軽量で人気の powerlevel10k
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git \
  ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
# .zshrc に
ZSH_THEME="powerlevel10k/powerlevel10k"
```

`p10k` の場合、初回起動時に対話設定が走ります。

## 5. 確認

```bash
echo $SHELL          # /usr/bin/zsh または /bin/zsh
echo $ZSH            # ~/.oh-my-zsh
```
