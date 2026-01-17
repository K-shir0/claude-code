---
description: タスク内容からブランチ名とタブ名を生成し、worktreeを分割してzellijで新しいタブを開き、Claude Codeを自動起動
argument-hint: <task-description | issue-url | file-path>
allowed-tools: [Bash, Read, WebFetch]
---

# Worktree with Branch and Zellij

タスク内容からブランチ名とタブ名（3-10文字、日本語可）を生成し、git worktreeを作成して、zellijで新しいタブを開き、Claude Codeを自動起動してタスクを開始する。

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

### Step 3.5: zellijタブ名の生成

タスク内容から短いタブ名を生成する:
- **長さ: 3-10文字**
- **日本語も使用可能**
- タスクの要点を表す短い名前
- 例:
  - "ユーザー認証機能を追加" → `認証` または `auth`
  - "バグ修正: ログイン失敗" → `ログイン` または `login-fix`
  - "ドキュメント更新" → `docs`
  - "リファクタリング: API層" → `API` または `refactor`

### Step 4: worktree作成

`wtp add -b <generated-branch-name>` を実行してworktreeを作成

worktreeのパスは `../worktrees/<branch-name>` になる
（wtpのデフォルト動作）

### Step 5: zellijで新しいタブを開き、Claude Codeを自動起動

以下のコマンドを順に実行:

1. タブ名を指定してzellijで新しいタブを開く:
```bash
zellij action new-tab --name "<tab-name>" --cwd <worktree-path>
```

2. タブ内でClaude Codeを自動起動し、タスク内容を引数として渡す:
```bash
zellij action write-chars "claude \"$ARGUMENTS\""
zellij action write 10
```

注意:
- `write 10` は Enter キーを送信する（10はEnterキーのキーコード）
- `$ARGUMENTS` はユーザーが指定した元の引数をそのまま使用
- これにより、タブ内で `claude "<タスク内容>"` が自動実行される

## 実行

上記の手順に従って、順番に実行してください。
各ステップの結果をユーザーに報告しながら進めてください。
