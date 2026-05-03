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

### 3. GNOME 入力ソースを Mozc 1 本に

**この構成のポイント**：US 配列の xkb と Mozc を別ソースで切り替えるのではなく、Mozc 1 本に統一し、Mozc 内部で「英字直接入力」と「ひらがな」を切り替える。
GNOME Wayland + IBus + Mozc では、複数ソース間切替がアクティブな IME のキーグラブと競合して不安定になりやすいため、この構成のほうが安定する。

```bash
# 入力ソースは Mozc のみ
gsettings set org.gnome.desktop.input-sources sources "[('ibus', 'mozc-jp')]"

# GNOME WM 側の source-switch ショートカットは無効化（1 ソースなので不要、競合除去）
gsettings set org.gnome.desktop.wm.keybindings switch-input-source "[]"
gsettings set org.gnome.desktop.wm.keybindings switch-input-source-backward "[]"

# IBus の engine on/off トリガを <Super>space + 半角/全角 キーに設定
# Control+space はエディタの補完ショートカットと衝突するため除外
gsettings set org.freedesktop.ibus.general.hotkey trigger "['<Super>space', 'Zenkaku_Hankaku']"

# IBus の source-switch トリガはクリア（1 ソースなので不要）
gsettings set org.freedesktop.ibus.general.hotkey triggers "[]"

# 反映
ibus restart
```

挙動：

| 状態 | インジケータ | 入力結果 |
|---|---|---|
| Mozc OFF（デフォルト） | `A_` | 直接英字 |
| Mozc ON | `あ` | ひらがな入力 → 変換 |

`Super+Space`（または JIS 物理キー `半角/全角`）で OFF ↔ ON を切替。

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

### 5. 反映確認

- **ログアウト → 再ログイン** で確実に反映（特に環境変数 / 新規プロセスでの IME 認識）
- インジケータが `A_`（OFF）/ `あ`（ON）に変わるか確認
- `Super+Space` でトグル

### 6. 確認コマンド（再ログイン後）

```bash
echo $GTK_IM_MODULE   # ibus
echo $QT_IM_MODULE    # ibus
echo $XMODIFIERS      # @im=ibus
ibus list-engine | grep -i mozc
gsettings get org.gnome.desktop.input-sources sources    # [('ibus', 'mozc-jp')] のみ
gsettings get org.freedesktop.ibus.general.hotkey trigger
```

### トラブルシューティング

#### Super+Space を押してもトグルしない

- IBus デーモンが新設定を読み込んでいない可能性 → `ibus restart` を再実行
- 既存セッションでキャッシュが残っている可能性 → ログアウト → 再ログイン
- IBus daemon の起動確認：`pgrep -af ibus-daemon`
- Mozc サーバの起動確認：`pgrep -af mozc_server`（無ければ最初の入力時に起動）

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
