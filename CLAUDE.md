# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## リポジトリの目的

個人用のClaude Codeプラグインマーケットプレイス。コマンド、サブエージェント、MCPサーバー、フック（Agent Skillsを含む）を一括配布・更新するための仕組み。プラグインはClaude Codeの機能を拡張する再利用可能な機能をパッケージ化する。

## プラグインアーキテクチャ

### プラグイン構成

各プラグインは `plugins/<plugin-name>/` 配下に配置され、以下で構成される。

- **`plugin.json`**: メタデータ（name・description・version・author・repository）である
- **`.mcp.json`**: MCPサーバー設定である。外部ツール統合を定義する
- **`commands/*.md`**: スラッシュコマンドである。呼び出されるとプロンプトに展開される（例：`/commit`）
- **`skills/` ディレクトリ**: Agent Skillsである。現在は未実装だがアーキテクチャとしてサポートする

### 利用可能なプラグイン

**common-dev-essentials**: 開発ワークフローの基本ユーティリティである。

- MCP: Serenaは `uvx` 経由でgitから取得し、セマンティックなコード分析・編集機能を提供する
- コマンド: `/common-dev-essentials:commit` はgit変更を分析してConventional Commits形式で日本語コミットを作成する

**general**: 汎用統合機能である。

- MCP: Perplexityは `npx perplexity-mcp` 経由でWeb検索・リサーチ機能を提供する

### MCPサーバー統合

MCPサーバーはClaudeと外部ツールを接続する。`.mcp.json` の設定形式は以下である。

```json
{
  "mcpServers": {
    "server-name": {
      "command": "実行ファイル",
      "args": ["引数1", "引数2"],
      "env": {
        "変数名": "${値}"
      }
    }
  }
}
```

環境変数の参照方法は以下である。

- `${PWD}` - カレントディレクトリ
- `${PROJECT_ROOT}` - プロジェクトルートパス
- `.env` ファイルの環境変数（例：`${PERPLEXITY_API_KEY}`）

## 開発環境

### コンテナセットアップ

DevContainerによる一貫した開発環境を提供する。

- ベース: `.devcontainer/` 内のDockerfile
- Features: Node 24とPython with uvとdirenv
- 自動インストール: コンテナ起動時にClaude Code CLI
- 拡張機能: Python、Ruff、mypy、Hadolint、Prettier

### 環境変数

direnvによる環境変数を管理する。

1. `.env.example` を `.env` にコピーする
2. 必要な値を入力する（例：`PERPLEXITY_API_KEY`）
3. direnvが `.envrc` 経由で自動的に変数をロードする（`init-direnv.sh` により許可される）

### コードフォーマット

PrettierをJSON用に設定済みである。

- タブ幅: 2スペース
- 行幅: 160文字
- シングルクォート: false
- 末尾カンマ: ES5
- VS Codeで保存時自動フォーマット有効

## スラッシュコマンドの扱い方

### コマンドファイル形式

コマンドはfrontmatter付きのMarkdownファイルである。

```markdown
---
allowed-tools: Bash(git add:*), Bash(git status:*)
description: 簡潔な説明
---

コマンドのプロンプト内容
```

`/commit` コマンドの特徴は以下である。

- 許可されたBashツール経由でgit操作する
- プロンプト内でシェルコマンド置換を使用する（例：`!`git status``）
- Conventional Commits形式に従い日本語で説明する
- コミットメッセージ内でAI関連の言及を避ける（Claude Code参照なしでありCo-Authored-Byなし）

## Gitワークフロー

- メインブランチ: `main`
- コミットはConventional Commits形式で日本語説明する
- タイプは `feat`・`fix`・`refactor`・`docs`・`test`・`chore`・`perf`・`style`・`build`・`ci` である
- 形式: `<type>: <日本語概要>` でありオプションで詳細説明を追加可能である

## プラグイン配布

プラグインは以下のように設計されている。

1. ディレクトリ内で自己完結する
2. セマンティックバージョニングでバージョン管理する
3. プラグインマーケットプレイス構造を通じてプロジェクト・チーム間で共有可能である
4. モジュラーコンポーネント（MCPサーバーとコマンドとスキル）で構成する
