---
allowed-tools: mcp__github__search_issues, mcp__github__issue_read, mcp__github__create_pull_request, mcp__github__list_pull_requests, mcp__github__pull_request_read, mcp__github__search_pull_requests, mcp__github__update_pull_request, mcp__github__get_commit, Bash(git:*)
description: Generate PR description and automatically create pull request on GitHub
---

## コンテキスト

- 現在のgit状態: !`git status`
- このPRの変更: !`git diff origin/main...HEAD`
- このPRのコミット: !`git log --oneline origin/main..HEAD`

## タスク

以下の順で、PRのdescriptionを生成して、PRを作成してください。

1. まず、PRのテンプレートの内容を確認してください。
    - .github/pull_request_template.md があればその内容を読んでください。なければ、PRの内容として以下としてください。
2. 現在のbranchに対応するPRが存在しているかを search_pull_requests と pull_request_read を使って確認してください
3. すでにPRが存在していれば、`update_pull_request`を使ってPRの内容を更新してください。存在していなければ、現在のbranchをpushし、 `create_pull_request`を使ってPRを作成してください。

### PR descriptionの内容

1. PRテンプレートの**正確な形式**に従って、日本語でPR説明を作成
2. このPRで行われた変更を視覚化する**Mermaid図**を追加

### 要件:

1. テンプレートの構造に正確に従うこと
2. すべてのコンテンツを日本語で記述すること
3. 具体的な実装の詳細を含めること
4. 具体的なテスト手順をリスト化すること
5. 以下を示すMermaid図を必ず含めること:
   - アーキテクチャの変更（ある場合）
   - データフローの修正
   - コンポーネント間の関係
   - 変更によって影響を受けるプロセスフロー
6. 包括的かつ簡潔であること

### Mermaid図のガイドライン:

- 適切な図のタイプを使用すること（フローチャート、シーケンス、クラスなど）
- 該当する場合は変更前後の状態を、対応を比較しやすい形で示すこと
- 新規または修正されたコンポーネントをハイライトすること
- 一貫したスタイルと色を使用すること
- PR説明の専用セクションに図を追加すること
