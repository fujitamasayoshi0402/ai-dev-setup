# AI駆動開発 セットアップガイド

AIコーディングエージェントを使った開発を始めるための、環境構築の手順とノウハウをまとめたリポジトリです。

特定のエージェントに依存しない構成にしています。Claude Code・Codex・Antigravity・GitHub Copilot など、**好きなものを1つ以上選んで**進めてください。

## 読む順番

3つの段階に分けています。**① まで読めば始められ、② まで読めば基本は完了**です。③ は必要になったときに読めば大丈夫です。

```mermaid
flowchart TD
    s1["① はじめる（全員）<br>00 → 01 → 02 または 03 → 04"]
    s2["② 使いこなす<br>05 → 06 → 07"]
    s3["③ 広げる（必要に応じて）<br>08・09・10・11"]
    s1 --> s2 --> s3
```

### ① はじめる（全員）

| # | ページ | 内容 |
|---|---|---|
| 00 | [はじめに: ファイル・フォルダ・ターミナルの基本](docs/00-basics.md) | パソコンの基本の言葉（ファイル・フォルダ・パス・GUI と CLI・ターミナル）。知っている人は飛ばしてOK |
| 01 | [AI エージェントの選び方](docs/01-choose-agent.md) | 仕組み・選び方の流れ・比較・それぞれの特徴 |
| 02 | [Mac セットアップ](docs/02-setup-mac.md) | 基本コース（Git・AI エージェント・Orca）→ ステップアップ → 開発者向け |
| 03 | [Windows セットアップ](docs/03-setup-windows.md) | 同上（Windows 版） |
| 04 | [Orca のセットアップ](docs/04-orca.md) | worktree（作業ごとのフォルダ）で、複数のエージェントを並行して動かす |

> 02（Mac）と 03（Windows）は、使う方だけ読めばOKです。セットアップは「基本コース」だけで始められます。

### ② 使いこなす

| # | ページ | 内容 |
|---|---|---|
| 05 | [フォルダの管理と Tips](docs/05-directory-and-tips.md) | フォルダの構成・雛形の使い方・変更の保存・AI への指示のコツ |
| 06 | [AGENTS.md の書き方](docs/06-agents-md.md) | AI への常設の指示書。エージェントごとに読むファイル・書き方のコツ・育て方 |
| 07 | [便利ツールの紹介と使い方](docs/07-tools.md) | エージェントの基本操作・効率化アプリ・タイピング練習・GitHub CLI など |

### ③ 広げる（必要に応じて）

| # | ページ | 内容 |
|---|---|---|
| 08 | [スキルの取り込みと共有](docs/08-skills.md) | 他のリポジトリのスキルを取り込む・自分のスキルを共有する |
| 09 | [複数のエージェントを使う](docs/09-multi-agent.md) | 使い分けのパターン・引き継ぎ・料金と利用枠の管理 |
| 10 | [相談ワークスペース](docs/10-consulting-workspace.md) | SEO・ブランディング・企画など、開発以外の仕事で知見を貯めて成果物を作る |
| 11 | [Obsidian（任意）](docs/11-obsidian-optional.md) | メモアプリ Obsidian と AI の組み合わせ |

> 各ページの上と下に、前後のページと目次へのリンクがあります。
> 図は GitHub 上と Obsidian でそのまま表示されます（Mermaid という書き方で作っています）。
> **プログラミング経験がなくても大丈夫です。**

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

1. ファイルやフォルダ、ターミナルに自信がなければ [00](docs/00-basics.md) を読む（10分）
2. [01](docs/01-choose-agent.md) で使う AI エージェントを1つ選ぶ（10分）
3. [02（Mac）](docs/02-setup-mac.md) または [03（Windows）](docs/03-setup-windows.md) の「基本コース」を済ませる（20〜30分）
4. [04](docs/04-orca.md) で Orca にフォルダを登録し、エージェントを起動して話しかけてみる
5. 慣れてきたら ② 使いこなす（05〜07）へ進む
6. 作ったものをバックアップ・共有したくなったら、セットアップの「ステップアップ」（GitHub）へ

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
├── .agents/skills/           # スキル（Codex・Antigravity・Copilot など）と取り込み元の記録
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
    ├── obsidian-vault/       # Obsidian でメモを管理するとき用
    └── consulting-workspace/ # 相談から成果物を作るとき用（SEO・ブランディングなど）
```

新しく何かを始めるときは、雛形をこのリポジトリの**隣**（`orca/projects/` の下）にコピーして使います。AI に「templates の new-project を使って my-app を作って」と頼むのがいちばん簡単です。詳しくは [templates/README.md](templates/README.md) を参照してください。

## 前提

- Mac または Windows のパソコン
- 使いたい AI エージェントのアカウント（どれか1つでOK）
  - 例: Claude（Claude Code）/ ChatGPT（Codex）/ Gemini（Google アカウント）（Antigravity）/ GitHub アカウント（GitHub Copilot）
  - 無料枠の有無や料金プランはエージェントごとに違うので、各公式サイトで確認してください
- GitHub アカウントは**任意**です（バックアップ・共有をしたくなったら作れば十分です）

## 更新について

ツールの進化が速いため、インストールコマンドは変わることがあります。うまくいかない場合は各ツールの公式ドキュメントを確認し、このリポジトリに PR で修正を送ってください。
