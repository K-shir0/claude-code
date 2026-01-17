# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## プロジェクト概要

`worktree-zellij` は Claude Code プラグインで、git worktree と Zellij ターミナルマルチプレクサを連携させて開発ワークフローを自動化する。

## プラグイン構成

```
.claude-plugin/
  plugin.json          # プラグインマニフェスト（名前、バージョン、説明）
commands/
  worktree:remove.md   # worktreeとブランチの削除コマンド
  worktree:with-branch-and-zellij.md  # worktree作成+Zellijタブ+Claude Code起動
.claude/
  settings.local.json  # 許可されるBashコマンドの設定
```

## コマンド定義の構造

コマンドは Markdown ファイルで定義され、YAML フロントマターを含む:

```yaml
---
description: コマンドの説明
argument-hint: <引数のヒント>
allowed-tools: [許可するツール]
---
```

## 外部ツール依存

- **wtp**: git worktree 管理ツール（worktree-plus）
  - `wtp add -b <branch>`: worktree 作成
  - `wtp list`: worktree 一覧
  - `wtp remove --with-branch <branch>`: worktree とブランチを削除
- **zellij**: ターミナルマルチプレクサ
  - `zellij action new-tab --name <name> --cwd <path>`: 新しいタブ作成
  - `zellij action write-chars`: タブにコマンド入力
  - `zellij action write 10`: Enter キー送信

## ブランチ命名規則

タスクからブランチ名を生成する際の規則:
- プレフィックス: `feature/`, `fix/`, `chore/`, `docs/`, `refactor/`
- 日本語は英語に変換
- スペースはハイフンに変換、小文字で統一
