# AI駆動開発 セットアップガイド

AIコーディングエージェントを使った開発を始めるための、環境構築の手順とノウハウをまとめたリポジトリです。

特定のエージェントに依存しない構成にしています。Claude Code・Codex・Antigravity など、**好きなものを1つ以上選んで**進めてください。

## 読む順番

| # | ドキュメント | 内容 |
|---|---|---|
| 01 | [Mac セットアップ](docs/01-setup-mac.md) | 基本コース（Git・AIエージェント・Orca）→ ステップアップ → 開発者向け |
| 02 | [Windows セットアップ](docs/02-setup-windows.md) | 基本コース（Git・AIエージェント・Orca）→ ステップアップ → 開発者向け |
| 03 | [便利ツールの紹介と使い方](docs/03-tools.md) | AIエージェントの比較と共通の使い方、CLI・効率化アプリ、タイピング練習 |
| 04 | [Orca のセットアップ](docs/04-orca.md) | worktree ベースで複数のエージェントを並行して動かす |
| 05 | [ディレクトリ管理と Tips](docs/05-directory-and-tips.md) | フォルダ構成・ブランチ運用・AIへの指示のコツ |
| 06 | [Obsidian（オプション）](docs/06-obsidian-optional.md) | ナレッジ管理とAIの組み合わせ |
| 07 | [スキルの取り込みと共有](docs/07-skills.md) | 他のリポジトリのスキルを取り込む・自分のスキルを共有する |
| 08 | [複数のエージェントを使うときの Tips](docs/08-multi-agent.md) | 使い分けのパターン・引き継ぎ・料金と利用枠の管理 |

> 01（Mac）と 02（Windows）は、使う方だけ読めばOKです。
> 図は GitHub 上と Obsidian でそのまま表示されます（Mermaid という書き方で作っています）。
> **プログラミング経験がなくても大丈夫です。** まずは各セットアップの「基本コース」だけ進めてください。

## AI と一緒に読み進める

このリポジトリを手元に置いて（下の「このリポジトリを手元に置く」）Orca に登録すると、**AI がガイド役**になります。`ai-dev-setup` でエージェントを起動して、次のように話しかけてみてください。

```
Mac で基本コースのセットアップを手伝って
```

```
Orca の使い方を、最初から順番に教えて
```

```
templates の new-project を使って、my-app という名前で新しいプロジェクトを作って
```

AI は `docs/` の内容に沿って、1〜2ステップずつ案内してくれます（ルールは `AGENTS.md` に書いてあります）。

## 最短ルート

1. OS別セットアップの「基本コース」を済ませる（20〜30分）
2. Orca でフォルダを登録し、エージェントを起動して話しかけてみる
3. 慣れてきたら Tips・タイピング練習・Obsidian へ進む
4. 作ったものをバックアップ・共有したくなったら「ステップアップ」（GitHub）へ

## このリポジトリを手元に置く（任意）

GitHub 上で読むだけでも進められます。手元に置くと、`templates/` の雛形を使ったり、AI にこのガイドを読ませて質問したりできます。**GitHub アカウントは不要です。**

推奨のディレクトリ構成（[05-directory-and-tips.md](docs/05-directory-and-tips.md)）に合わせて、`orca/projects/` の下に置きます。

Mac（Git を入れたあと）：

```zsh
mkdir -p ~/orca/projects ~/orca/workspaces
cd ~/orca/projects
git clone https://github.com/fujitamasayoshi0402/ai-dev-setup.git
```

Windows（Git for Windows を入れたあと）：

```powershell
mkdir C:\dev\orca\projects, C:\dev\orca\workspaces -Force
cd C:\dev\orca\projects
git clone https://github.com/fujitamasayoshi0402/ai-dev-setup.git
```

> Git をまだ入れていない場合は、GitHub のページ右上の「Code」→「Download ZIP」でダウンロードし、展開したフォルダを `ai-dev-setup` という名前で上の場所に置いても構いません。

### 最新版にする

このガイドは随時更新されます。`ai-dev-setup` でエージェントに「このガイドを最新にして」と頼むと、最新版を取り込みます。自分で行う場合は次のとおりです。

```zsh
cd ~/orca/projects/ai-dev-setup
git pull
```

```powershell
cd C:\dev\orca\projects\ai-dev-setup
git pull
```

> 雛形からコピーして作ったプロジェクトは、更新しても影響を受けません。ZIP でダウンロードした場合は、新しい ZIP を取り直してください。

## このリポジトリの構成

このリポジトリ自体を、AI駆動開発のディレクトリ構成のお手本にしています。

```
ai-dev-setup/
├── README.md                 # 目次（このファイル）
├── AGENTS.md                 # AI エージェント共通の指示書
├── CLAUDE.md                 # Claude Code 用（AGENTS.md を読み込むだけ）
├── .agents/skills/           # スキル（Codex・Antigravity など）と取り込み元の記録
├── .claude/
│   ├── settings.json         # Claude Code の許可・禁止コマンド（チーム共有）
│   └── skills/               # スキル（Claude Code 用。中身は .agents/skills と同じ）
├── .github/
│   └── pull_request_template.md
├── .gitignore                # 秘密情報・個人設定を除外
├── .gitattributes            # 改行コードを LF に統一
├── .editorconfig             # 文字コード・インデントを統一
├── docs/                     # 本文（番号順に読む）
└── templates/                # 雛形（コピーして使う。ここでは作業しない）
    ├── new-project/          # アプリ・ツールを作るとき用
    └── obsidian-vault/       # Obsidian でメモを管理するとき用
```

新しく何かを始めるときは、雛形をこのリポジトリの**隣**（`orca/projects/` の下）にコピーして使います。AI に「templates の new-project を使って my-app を作って」と頼むのがいちばん簡単です。詳しくは [templates/README.md](templates/README.md) を参照してください。

## 前提

- Mac または Windows のパソコン
- 使いたい AI エージェントのアカウント（どれか1つでOK）
  - 例: Claude（Claude Code）/ ChatGPT（Codex）/ Google アカウント（Antigravity）
  - 無料枠の有無や料金プランはエージェントごとに違うので、各公式サイトで確認してください
- GitHub アカウントは**任意**です（バックアップ・共有をしたくなったら作れば十分です）

## 更新について

ツールの進化が速いため、インストールコマンドは変わることがあります。うまくいかない場合は各ツールの公式ドキュメントを確認し、このリポジトリに PR で修正を送ってください。
