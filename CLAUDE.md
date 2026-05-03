# CLAUDE.md

このリポジトリで Claude（Claude Code 等）が作業するときのコンテキストです。

## リポジトリの目的

新しいマシン（macOS / Debian/Ubuntu）の環境構築手順を一元管理する個人用ナレッジベース。
Claude が `docs/` を読むことで、セットアップを半自動でガイド・実行できる状態を目指す。

実体は **手順書（マニュアル）** であり、各ファイルは「コピペで動く」ことを最優先する。

## ファイル構成と役割

```
setup_my_env/
├── README.md          # 人間向けの使い方
├── CLAUDE.md          # このファイル（Claude 向けコンテキスト）
├── master.md          # 全項目チェックリスト（マスター）
├── current.md         # 各マシンの進捗記録（gitignore 対象、master.md からコピーして使う）
└── docs/
    ├── common/        # OS 共通の手順（git, claude, shell, cursor, tailscale, discord, ...）
    ├── macos/         # macOS 固有
    └── debian/        # Debian/Ubuntu 固有（apt, dev-tools, system, ...）
```

- `master.md` … チェックリスト本体。新しい手順を追加したら必ずここにも項目追加。
- `current.md` … `cp master.md current.md` で作る作業用。**gitignore されている**。リポジトリには含めない。
- `docs/<category>/<topic>.md` … 各トピックの詳細手順。

## ドキュメントを追加・編集するときのルール

1. **配置場所の判断**
   - OS をまたぐツール（Cursor / Discord / Tailscale 等）→ `docs/common/<name>.md`
   - OS 固有のシステム設定 → `docs/<os>/system.md` 等
   - パッケージマネージャ経由のセットアップ → `docs/<os>/apt.md` や `dev-tools.md`

2. **手順は「コピペで動く」形で書く**
   - シェルブロックは前提（OS / 権限 / 既存環境）が分かるように書く
   - 確認コマンドを必ず添える（インストール後の `--version` 等）

3. **`master.md` のチェックリストを必ず同期更新**
   - 新規手順を追加したら、対応する項目を `master.md` の適切なセクションに追加。
   - 既存項目の手順を変更しただけなら master.md 編集は不要。

4. **README.md のファイル構成図も実体と一致させる**
   - 新規ファイルを追加したら README の tree も更新。

5. **commit メッセージは英語**
   - 既存の commit に合わせて conventional 風（`docs:`, `fix:` 等）を推奨。

## 環境前提（このリポジトリのオーナーのデフォルト）

- 主に Debian 13 / GNOME Wayland 環境で運用中
- JIS キーボードを **US 配列にして** 使用
- 日本語入力は **IBus + Mozc**
- シェルは zsh（Oh My Zsh 想定）
- エディタは Cursor / Claude Code
- Git の identity: `KogoTakuma` / `1358takuma@gmail.com`
- リモート: `git@github.com:KogoTakuma/setup_my_env.git`

## トーン・言語

- ドキュメント本文は **日本語**（既存ドキュメントに合わせる）
- コードブロック内のコメントは英語/日本語どちらでも可（短く）
- ユーザー（オーナー）との会話は基本日本語

## やってはいけないこと

- `current.md` を repo に commit する（gitignore 対象）
- 動作未確認の手順をそれと明記せずドキュメント化する
- マスターチェックリストを更新せずに新規 doc を追加する
- 既存の commit を amend / 履歴を書き換える
