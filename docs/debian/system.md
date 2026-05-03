# システム設定（Debian/Ubuntu）

GNOME Wayland 環境を前提としています。

## キーボードレイアウト（JIS → US）

JIS 配列のキーボードを US 配列として認識させる設定。
システム全体（コンソール・ログイン画面）と GNOME セッションの両方を変更します。

### 1. システム全体（`/etc/default/keyboard`）

```bash
sudo sed -i 's/XKBLAYOUT="jp"/XKBLAYOUT="us"/' /etc/default/keyboard
sudo dpkg-reconfigure -f noninteractive keyboard-configuration
```

### 2. GNOME 入力ソース

```bash
gsettings set org.gnome.desktop.input-sources sources "[('xkb', 'us')]"
```

GNOME 側の変更は即時反映されます。ログイン画面（GDM）など全体に反映するには
ログアウト → 再ログイン、または再起動。

### 確認

```bash
# システム側
grep XKBLAYOUT /etc/default/keyboard
localectl status

# GNOME 側
gsettings get org.gnome.desktop.input-sources sources
```

---

## 日本語入力（IBus + Mozc）

GNOME はデフォルトで IBus を使用します。Mozc エンジンを追加して日本語入力を有効化します。

### 1. インストール

```bash
sudo apt update
sudo apt install -y ibus-mozc
```

### 2. IBus デーモン再起動（Mozc エンジン認識のため）

```bash
ibus restart
```

認識されたか確認：

```bash
ibus list-engine | grep -i mozc
# mozc-jp - Mozc が出れば OK
```

### 3. GNOME 入力ソースに Mozc を追加

US キーボードと Mozc の両方を入力ソースとして登録：

```bash
gsettings set org.gnome.desktop.input-sources sources "[('xkb', 'us'), ('ibus', 'mozc-jp')]"
```

### 4. IM 環境変数の設定（重要）

これを設定しないと「アプリによって切替が効いたり効かなかったり」する症状が出ます。
原因は GTK アプリや Electron 系（VS Code / Cursor / Discord / Slack 等）が
`GTK_IM_MODULE` を見て IBus 経路を判定するため。

#### im-config で IBus を選択

```bash
im-config -n ibus
```

これで `~/.xinputrc` が生成されます。

#### systemd user environment にも登録（GNOME Wayland で確実に効かせる）

```bash
mkdir -p ~/.config/environment.d
cat > ~/.config/environment.d/im.conf <<'EOF'
GTK_IM_MODULE=ibus
QT_IM_MODULE=ibus
XMODIFIERS=@im=ibus
EOF
```

### 5. 反映 & 切替

- **ログアウト → 再ログイン** で確実に反映（特に環境変数 / 新規プロセスでの IME 認識）
- 切替ショートカット：**Super + Space**
- 上部バー右上に `EN` / `あ` のインジケーターが表示される
- Mozc 利用中の細かい挙動（半角/全角・変換キー等）は Mozc のショートカットで設定

### 6. 確認（再ログイン後）

```bash
echo $GTK_IM_MODULE   # ibus
echo $QT_IM_MODULE    # ibus
echo $XMODIFIERS      # @im=ibus
ibus list-engine | grep -i mozc
```

### トラブルシューティング

#### Super+Space が「効くとき効かないとき」がある

GNOME WM の切替バインドと IBus 内部ホットキーが両方とも `<Super>space` に
bind されているため、押すたびにどちらが拾うかが不定になりレース状態が発生する。
IBus 側を空にして GNOME 一本に統一する：

```bash
gsettings set org.freedesktop.ibus.general.hotkey triggers "[]"
ibus restart
```

#### 切替表示は出るが入力が切り替わらない

- 上記「IM 環境変数の設定」が済んでいるか確認 → ログアウト/再ログイン
- `ibus restart` を再実行
- `pgrep -af ibus-daemon` で daemon の起動状況を確認

#### Electron アプリ（VS Code / Cursor / Discord / Slack 等）で効かない場合

Wayland ネイティブで起動すると IME 連携にフラグが必要：

```bash
# 一時テスト
<app> --enable-features=UseOzonePlatform --ozone-platform=wayland --enable-wayland-ime
```

永続化したい場合は各アプリの flags ファイル（`~/.config/<app>-flags.conf`）に：

```
--enable-wayland-ime
--ozone-platform-hint=auto
```

または `.desktop` ファイルの `Exec=` に同じフラグを追加。
