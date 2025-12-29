---
description: タスク内容からブランチ名を推測し、worktreeを分割してzellijで新しいタブを開く
argument-hint: <task-description | issue-url | file-path>
allowed-tools: [Bash, Read, WebFetch]
---

# Worktree with Branch and Zellij

タスク内容からブランチ名を生成し、git worktreeを作成して、zellijで新しいタブを開く。

## 引数

ユーザーが指定した引数: $ARGUMENTS

## 実行手順

### Step 1: 引数の解析

引数 `$ARGUMENTS` を解析して入力タイプを判定する:
- `https://` または `http://` で始まる → Issue URLとして処理
- `.md` を含む、または `/` で始まる → ファイルパスとして処理
- それ以外 → タスク説明として処理

### Step 2: タスク内容の取得

入力タイプに応じてタスク内容を取得する:

**Issue URLの場合:**
`gh issue view <url>` または `gh pr view <url>` でタイトルと本文を取得

**ファイルパスの場合:**
Readツールでファイル内容を読み込む

**タスク説明の場合:**
引数をそのままタスク内容として使用

### Step 3: ブランチ名の生成

タスク内容から適切なブランチ名を生成する:
- 命名規則: `feature/xxx`, `fix/xxx`, `chore/xxx`, `docs/xxx`, `refactor/xxx`
- 日本語がある場合は英語に変換
- スペースはハイフンに変換
- 小文字で統一
- 例: "ユーザー認証機能を追加" → `feature/add-user-authentication`

### Step 4: worktree作成

`wtp add -b <generated-branch-name>` を実行してworktreeを作成

worktreeのパスは `../worktrees/<branch-name>` になる
（wtpのデフォルト動作）

### Step 5: zellijで新しいタブを開く

以下のコマンドを実行:
```bash
zellij --session $ZELLIJ_SESSION_NAME action new-tab --cwd <worktree-path>
```

## 実行

上記の手順に従って、順番に実行してください。
各ステップの結果をユーザーに報告しながら進めてください。
