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

## 前提

- GitHub アカウント
- 使いたい AI エージェントのアカウント（どれか1つでOK）
  - 例: Claude（Claude Code）/ ChatGPT（Codex）/ Google アカウント（Antigravity）
  - 無料枠の有無や料金プランはエージェントごとに違うので、各公式サイトで確認してください

## 更新について

ツールの進化が速いため、インストールコマンドは変わることがあります。うまくいかない場合は各ツールの公式ドキュメントを確認し、このリポジトリに PR で修正を送ってください。
