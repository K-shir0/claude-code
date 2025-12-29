---
description: worktreeとブランチを削除する
argument-hint: <branch-name>
allowed-tools: [Bash]
---

# Worktree Remove

worktreeとブランチを同時に削除する。

## 引数

ユーザーが指定したブランチ名: $ARGUMENTS

## 実行手順

### Step 1: 現在のworktree一覧を確認

`wtp list` を実行して現在のworktree一覧を表示する。

### Step 2: worktreeとブランチを削除

引数で指定されたブランチ名を使って、以下のコマンドを実行:

```bash
wtp remove --with-branch <branch-name>
```

このコマンドは:
- worktreeディレクトリを削除
- 関連するブランチも同時に削除

### Step 3: 削除の確認

`wtp list` を再度実行して、削除が完了したことを確認する。

## 実行

上記の手順に従って、順番に実行してください。
