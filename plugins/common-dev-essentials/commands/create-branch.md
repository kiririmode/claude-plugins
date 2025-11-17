---
allowed-tools: Bash(git status:*), Bash(git fetch:*), Bash(git diff:*), Bash(git pull:*), Bash(git switch -c:*), Bash(git symbolic-ref:*), Bash(git rev-parse:*)
description: Create a new Git branch following Conventional Branch naming conventions
---

このコマンドは、一般的に推奨される手順を踏まえてGitのbranchを作成する。branch名はConventional Branchに従う。

## Conventional Branchとは

Conventional Branchは、Gitブランチの構造化された標準的な命名規則を指す。ブランチをより読みやすく実用的にすることを目的としている。いくつかのブランチプレフィックスを提案しているが、独自の命名規則を指定できる。一貫した命名規則により、タイプ別にブランチを識別しやすくなる。

### ブランチ命名プレフィックス

ブランチ仕様は以下のプレフィックスをサポートし、次のように構造化される。

`<type>/<description>`

- `main`: メイン開発ブランチ (例: main, master, develop)
- `feature/` (または `feat/`): 新機能用 (例: `feature/add-login-page`, `feat/add-login-page`)
- `fix/`: バグ修正用 (例: `fix/fix-header-bug`, `fix/header-bug`)
- `hotfix/`: 緊急修正用 (例: `hotfix/security-patch`)
- `release/`: リリース準備用 (例: `release/v1.2.0`)
- `chore/`: 依存関係更新やドキュメント更新などの非コードタスク用 (例: `chore/update-dependencies`)

### 基本ルール

- 小文字英数字・ハイフン・ドットを使用: 常に小文字(a-z)、数字(0-9)、ハイフン(-)で単語を区切る。特殊文字、アンダースコア、スペースは避ける。
  リリースブランチでは、バージョン番号の表記にドット(.)を使用できる (例: release/v1.2.0)。
- 連続したハイフンやドットを使用しない: ハイフンとドットが連続する記述 (例: `feature/new--login`, `release/v1.-2.0`) は避ける。
  descriptionの先頭や末尾への記述 (例: `feature/-new-login`, `release/v1.2.0.`) も避ける。
- 明確かつ簡潔に: ブランチ名は説明的でありながら簡潔にし、作業の目的を明確に示す。
- チケット番号を含める: 該当する場合、プロジェクト管理ツールのチケット番号を含めて追跡を容易にする。例えば、チケットissue-123の場合、ブランチ名は `feature/issue-123-new-login` とする。

## 実行手順

以下の情報を収集してブランチ作成に必要なコンテキストを把握する。

1. リモートリポジトリのデフォルトブランチを確認: !`git symbolic-ref refs/remotes/origin/HEAD`
2. 現在のブランチ名を確認: !`git rev-parse --abbrev-ref HEAD`
3. 現在の作業ディレクトリの状態を確認: !`git status`
4. 現在のブランチのコミット履歴を確認: `git log --oneline origin/[デフォルトブランチ]..HEAD`
5. 必要に応じて、デフォルトブランチとの差分を確認: `git diff origin/[デフォルトブランチ]...HEAD`

上記のコンテキストとユーザとの対話内容を踏まえて、Conventional Branch命名規則に従った適切なブランチ名を決定する。

## タスク

`git switch -c [ブランチ名]` を実行し、新しいブランチを作成して移動する。

## ブランチ名の要件

1. Conventional Branch命名規則に従うこと（`<type>/<description>` の形式）
2. 小文字英数字とハイフンのみを使用すること
3. 簡潔かつ明確に作業内容を表すこと
4. 該当する場合はチケット番号を含めること（例: `feat/issue-123-description`）
