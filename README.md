# AI駆動開発 セットアップガイド

AIコーディングエージェントを使った開発を始めるための、環境構築の手順とノウハウをまとめたリポジトリです。

特定のエージェントに依存しない構成にしています。Claude Code・Codex・Antigravity など、**好きなものを1つ以上選んで**進めてください。

## 読む順番

| # | ドキュメント | 内容 |
|---|---|---|
| 1 | [Mac セットアップ](docs/01-setup-mac.md) | 基本コース（Git・AIエージェント・Orca）→ ステップアップ → 開発者向け |
| 1 | [Windows セットアップ](docs/02-setup-windows.md) | 基本コース（Git・AIエージェント・Orca）→ ステップアップ → 開発者向け |
| 2 | [便利ツールの紹介と使い方](docs/03-tools.md) | AIエージェントの比較と共通の使い方、CLI・効率化アプリ、タイピング練習 |
| 3 | [Orca のセットアップ](docs/04-orca.md) | worktree ベースで複数のエージェントを並行して動かす |
| 4 | [ディレクトリ管理と Tips](docs/05-directory-and-tips.md) | フォルダ構成・ブランチ運用・AIへの指示のコツ |
| 5 | [Obsidian（オプション）](docs/06-obsidian-optional.md) | ナレッジ管理とAIの組み合わせ |

> Mac と Windows の手順は、使う方だけ読めばOKです。
> **プログラミング経験がなくても大丈夫です。** まずは各セットアップの「基本コース」だけ進めてください。

## 最短ルート

1. OS別セットアップの「基本コース」を済ませる（20〜30分）
2. Orca でフォルダを登録し、エージェントを起動して話しかけてみる
3. 慣れてきたら Tips・タイピング練習・Obsidian へ進む
4. 作ったものをバックアップ・共有したくなったら「ステップアップ」（GitHub）へ

## このリポジトリを手元に置く（任意）

GitHub 上で読むだけでも進められます。手元に置くと、`templates/` の雛形を使ったり、AI にこのガイドを読ませて質問したりできます。**GitHub アカウントは不要です。**

推奨のディレクトリ構成（[05-directory-and-tips.md](docs/05-directory-and-tips.md)）に合わせて、`orca/projects/` の下に置きます。

```zsh
# Mac（Git を入れたあと）
mkdir -p ~/orca/projects ~/orca/workspaces
cd ~/orca/projects
git clone https://github.com/fujitamasayoshi0402/ai-dev-setup.git
```

```powershell
# Windows（Git for Windows を入れたあと）
mkdir C:\dev\orca\projects, C:\dev\orca\workspaces -Force
cd C:\dev\orca\projects
git clone https://github.com/fujitamasayoshi0402/ai-dev-setup.git
```

> Git をまだ入れていない場合は、GitHub のページ右上の「Code」→「Download ZIP」でダウンロードし、展開したフォルダを `ai-dev-setup` という名前で上の場所に置いても構いません。

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

- Mac または Windows のパソコン
- 使いたい AI エージェントのアカウント（どれか1つでOK）
  - 例: Claude（Claude Code）/ ChatGPT（Codex）/ Google アカウント（Antigravity）
  - 無料枠の有無や料金プランはエージェントごとに違うので、各公式サイトで確認してください
- GitHub アカウントは**任意**です（バックアップ・共有をしたくなったら作れば十分です）

## 更新について

ツールの進化が速いため、インストールコマンドは変わることがあります。うまくいかない場合は各ツールの公式ドキュメントを確認し、このリポジトリに PR で修正を送ってください。
