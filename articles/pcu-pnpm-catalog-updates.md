---
title: "pnpm catalogの依存関係を一括更新するCLIツール「pcu」を作った"
emoji: "📦"
type: "tech"
topics: ["pnpm", "nodejs", "cli", "npm"]
published: true
---

## pnpm catalog、使ってますか？

pnpm v9 で導入された **catalog** 機能は、モノレポにおける依存関係のバージョン管理を劇的に改善しました。`pnpm-workspace.yaml` にカタログを定義すれば、各パッケージの `package.json` では `catalog:` プロトコルで統一管理できます。

```yaml
# pnpm-workspace.yaml
catalog:
  react: ^19.0.0
  typescript: ^5.8.0
  vite: ^6.2.0
```

ところが、**カタログ内の依存関係を最新版に更新する公式ツールがない**。`ncu`（npm-check-updates）は `package.json` にしか対応しておらず、`pnpm-workspace.yaml` の catalog は管轄外です。

毎回手動で npm のバージョンを確認して書き換える……？面倒すぎる。

そこで **pcu**（pnpm-catalog-updates）を作りました。

https://github.com/yldm-tech/pnpm-catalog-updates

## pcuとは

pnpm workspace catalog 専用の依存関係更新ツールです。`ncu` の catalog 版と思ってもらえれば。

```bash
npm install -g pcu
```

![pcu screenshot](https://github.com/user-attachments/assets/f05a970e-c58c-44f1-b3f1-351ae30b4a35)

## 基本的な使い方

### 更新チェック

```bash
# catalogの更新をチェック
pcu check
# または
pcu -c
```

現在のバージョンと最新バージョンを比較し、更新可能なパッケージを一覧表示します。

### 一括更新

```bash
# すべて更新
pcu update
# または
pcu -u
```

### インタラクティブモード

```bash
pcu -i
```

更新したいパッケージを選択式で選べます。「全部上げたいわけじゃないけど、いくつかは上げたい」というケースに便利。

## 特徴的な機能

### 🤖 AI分析

```bash
# AI分析を有効にして更新チェック
pcu check --ai

# AIプロバイダーの設定
pcu ai
```

Claude、Gemini、Codex CLI と連携して、更新内容の影響分析やリスク評価を自動で行います。Breaking change がありそうなパッケージを事前に教えてくれるので、安心してアップデートできます。

### 🔒 セキュリティスキャン

```bash
pcu security
```

カタログ内の依存関係に既知の脆弱性がないかチェック。問題があれば自動修正も可能。

### 🎨 テーマ

```bash
pcu theme
```

ターミナルの見た目をカスタマイズできます。地味に嬉しい。

### 🌐 多言語対応

英語・中国語・日本語・韓国語・スペイン語・ドイツ語・フランス語の7言語に対応しています。

## 設定ファイル

プロジェクトルートに `.pcurc.json` を置くことで、挙動をカスタマイズできます。

```json
{
  "defaults": {
    "target": "latest",
    "createBackup": true,
    "format": "table"
  },
  "include": [],
  "exclude": ["react", "react-dom"]
}
```

`exclude` で特定パッケージを更新対象から外せるので、メジャーバージョンを固定したいものがある場合に便利です。

## ncu との違い

| 機能 | ncu | pcu |
|------|-----|-----|
| package.json の更新 | ✅ | ❌ |
| pnpm catalog の更新 | ❌ | ✅ |
| インタラクティブモード | ✅ | ✅ |
| AI 分析 | ❌ | ✅ |
| セキュリティスキャン | ❌ | ✅ |
| テーマ | ❌ | ✅ |

棲み分けは明確で、**`ncu` は package.json、`pcu` は pnpm catalog**。併用が基本です。

## インストールと実行

```bash
# グローバルインストール
npm install -g pcu

# または npx で直接実行
npx pcu -c

# ワークスペースの初期化（初回のみ）
pcu init
```

Node.js 22 以上が必要です。

## まとめ

pnpm catalog はモノレポの依存管理を大幅に簡素化する素晴らしい機能ですが、更新作業だけが手動のままでした。`pcu` を使えば、catalog の更新チェック → 選択 → 適用を数秒で完了できます。

pnpm + monorepo 構成で開発している方は、ぜひ試してみてください。

📦 **npm:** https://www.npmjs.com/package/pcu
🐙 **GitHub:** https://github.com/yldm-tech/pnpm-catalog-updates
📖 **ドキュメント:** https://pcu-cli.dev

Star ⭐ いただけると励みになります！
