# AI駆動開発 セットアップガイド

このリポジトリは、AIコーディングエージェント（Claude Code / Codex など）を使った開発を始めるための手順とノウハウをまとめたものです。

## 読む順番

| # | ドキュメント | 内容 |
|---|---|---|
| 1 | [Mac セットアップ](docs/01-setup-mac.md) | Homebrew / Git / GitHub CLI / Node / Claude Code / Codex の導入 |
| 1 | [Windows セットアップ](docs/02-setup-windows.md) | winget / Git for Windows / Claude Code / Codex の導入 |
| 2 | [便利ツールの紹介と使い方](docs/03-tools.md) | Claude Code・Codex・gh・mise などの使い方 |
| 3 | [Orca のセットアップ](docs/04-orca.md) | worktree ベースで複数エージェントを並行運用する |
| 4 | [ディレクトリ管理と Tips](docs/05-directory-and-tips.md) | フォルダ構成・ブランチ運用・AIへの指示のコツ |
| 5 | [Obsidian（オプション）](docs/06-obsidian-optional.md) | ナレッジ管理とAIの組み合わせ |

> Mac と Windows はどちらか一方だけ読めばOKです。

## 最短ルート

1. OS別のセットアップを済ませる（30〜60分）
2. `claude` を起動してログインできることを確認
3. Orca を入れてリポジトリを登録し、worktree を1つ作ってエージェントを起動
4. 慣れてきたら Tips と Obsidian を読む

## 前提

- GitHub アカウント
- Claude（Pro / Max / Team など）または Anthropic Console のアカウント
- （任意）ChatGPT アカウント（Codex を使う場合）

## 更新について

ツールの進化が速いため、インストールコマンドは変わることがあります。うまくいかない場合は各ツールの公式ドキュメントを確認し、このリポジトリへ PR で修正してください。
