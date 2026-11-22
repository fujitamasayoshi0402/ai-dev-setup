# AI駆動開発 セットアップガイド

AIコーディングエージェントを使った開発を始めるための、環境構築の手順とノウハウをまとめたリポジトリです。

特定のエージェントに依存しない構成にしています。Claude Code・Codex・Antigravity など、**好きなものを1つ以上選んで**進めてください。

## 読む順番

| # | ドキュメント | 内容 |
|---|---|---|
| 1 | [Mac セットアップ](docs/01-setup-mac.md) | Homebrew / Git / GitHub CLI / Node / AIエージェント / 便利アプリ |
| 1 | [Windows セットアップ](docs/02-setup-windows.md) | winget / Git for Windows / Node / AIエージェント |
| 2 | [便利ツールの紹介と使い方](docs/03-tools.md) | AIエージェントの比較と共通の使い方、CLI・効率化アプリ、タイピング練習 |
| 3 | [Orca のセットアップ](docs/04-orca.md) | worktree ベースで複数のエージェントを並行して動かす |
| 4 | [ディレクトリ管理と Tips](docs/05-directory-and-tips.md) | フォルダ構成・ブランチ運用・AIへの指示のコツ |
| 5 | [Obsidian（オプション）](docs/06-obsidian-optional.md) | ナレッジ管理とAIの組み合わせ |

> Mac と Windows の手順は、使う方だけ読めばOKです。

## 最短ルート

1. OS別のセットアップを済ませる（30〜60分）
2. AIエージェントを1つ選んでインストールし、ログインできることを確認する
3. Orca を入れてリポジトリを登録し、worktree を1つ作ってエージェントを起動する
4. 慣れてきたら Tips・タイピング練習・Obsidian へ進む

## はじめ方（クローン）

推奨のディレクトリ構成（[05-directory-and-tips.md](docs/05-directory-and-tips.md)）に合わせて、`~/orca/projects/` の下にクローンします。

```zsh
# Mac
mkdir -p ~/orca/projects ~/orca/workspaces
cd ~/orca/projects
gh repo clone fujitamasayoshi0402/ai-dev-setup
```

```powershell
# Windows
mkdir C:\dev\orca\projects, C:\dev\orca\workspaces -Force
cd C:\dev\orca\projects
gh repo clone fujitamasayoshi0402/ai-dev-setup
```

> `gh` をまだ入れていない場合は、先に OS 別のセットアップを済ませてください。GitHub 上でこのページを読みながら進めても構いません。

## このリポジトリの構成

このリポジトリ自体を、AI駆動開発のディレクトリ構成のお手本にしています。

```
ai-dev-setup/
├── README.md                 # 目次（このファイル）
├── AGENTS.md                 # AI エージェント共通の指示書
├── CLAUDE.md                 # Claude Code 用（AGENTS.md を読み込むだけ）
├── .claude/settings.json     # Claude Code の許可・禁止コマンド（チーム共有）
├── .github/
│   └── pull_request_template.md
├── .gitignore                # 秘密情報・個人設定を除外
├── .gitattributes            # 改行コードを LF に統一
├── .editorconfig             # 文字コード・インデントを統一
├── docs/                     # 本文（番号順に読む）
└── templates/
    └── new-project/          # 新しいプロジェクトにコピーして使う雛形
```

新しいプロジェクトを始めるときは `templates/new-project/` の中身をコピーし、`AGENTS.md` の「〇〇」をプロジェクトに合わせて書き換えます（手順は [05-directory-and-tips.md](docs/05-directory-and-tips.md#2-リポジトリ内の構成例)）。

## 前提

- GitHub アカウント
- 使いたい AI エージェントのアカウント（どれか1つでOK）
  - 例: Claude（Claude Code）/ ChatGPT（Codex）/ Google アカウント（Antigravity）
  - 無料枠の有無や料金プランはエージェントごとに違うので、各公式サイトで確認してください

## 更新について

ツールの進化が速いため、インストールコマンドは変わることがあります。うまくいかない場合は各ツールの公式ドキュメントを確認し、このリポジトリに PR で修正を送ってください。
