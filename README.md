# 環境構築ガイド

新しいパソコンをセットアップする際の環境構築手順をまとめたリポジトリです。  
Claude AI が読み込んで、セットアップ作業を自動化・アシストできるようにドキュメント化しています。

## 使い方

1. `master.md` - セットアップ項目のマスターリスト（すべてのチェックボックス）
2. `current.md` - セットアップ進捗を記録するファイル（master.md をコピーして使用）
3. `docs/` - 各セットアップ手順の詳細ドキュメント

### 初めに

```bash
# currentファイルを作成
cp master.md current.md
```

その後、`current.md` を編集しながらセットアップを進めます。

## ファイル構成

```
setup_my_env/
├── README.md              # このファイル
├── master.md              # マスターリスト
├── current.md             # 進捗記録（gitignore対象）
├── .gitignore
└── docs/
    ├── common/            # 共通セットアップ
    │   ├── git.md
    │   ├── claude.md
    │   ├── shell.md
    │   ├── cursor.md
    │   ├── tailscale.md
    │   └── discord.md
    ├── macos/             # macOS 固有
    │   ├── homebrew.md
    │   ├── dev-tools.md
    │   └── system.md
    └── debian/            # Debian/Ubuntu 固有
        ├── apt.md
        ├── dev-tools.md
        └── system.md
```

## 目的

Claude が `docs/` に格納されたドキュメント群を読み込むことで、
ほぼ全自動でセットアップスクリプトを実行・ガイダンスができる状態を目指しています。
