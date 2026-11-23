<!-- nav-top -->
[目次](../README.md#読む順番) ＞ ① はじめる ＞ **02 Mac セットアップ**
<!-- /nav-top -->

# Mac セットアップ

macOS（Apple Silicon / Intel）で AI 駆動開発を始めるための手順です。**プログラミング経験がなくても進められる**ように、3つのコースに分けています。

| コース | 対象 | 入れるもの |
|---|---|---|
| [基本コース](#基本コース全員) | 全員 | Git ＋ AIエージェント1つ ＋ Orca |
| [ステップアップ](#ステップアップ) | バックアップ・共有もしたい人 | Homebrew、GitHub アカウント、GitHub CLI |
| [開発者向け](#開発者向け) | Web アプリなどを本格的に作る人 | Node.js / mise、ripgrep / jq、エディタ |

まずは **基本コースだけ** で大丈夫です。足りないものが出てきたら、その時点で AI に「〇〇を入れて」と頼めば入れてくれます。

## ターミナルの開き方

> 「ファイル」「フォルダ」「ターミナル」などの言葉に自信がない場合は、先に [はじめに: ファイル・フォルダ・ターミナルの基本](00-basics.md) を読んでください。

`⌘ + Space` で Spotlight を開き、「ターミナル」と入力して Enter を押します。以降のコマンドは、ここにコピー＆ペーストして Enter で実行します。

---

## 基本コース（全員）

### 1. Git を入れる

Git は「ファイルの変更履歴を残す仕組み」です。**AI が変更を壊しても元に戻せる、取り消しボタン**の役割をします。Orca が1タスクごとに作業フォルダを分ける機能（worktree）も Git を使っています。

```zsh
xcode-select --install
```

ダイアログが出たら「インストール」を押します（数分かかります）。終わったら確認します。

```zsh
git --version
```

`git version 2.xx` のように表示されれば OK です。

続けて、変更履歴に残す名前を設定します（メールアドレスは、後で GitHub を使うときに変更できます）。

```zsh
git config --global user.name "あなたの名前"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
```

> Git のコマンドを覚える必要はありません。「ここまでの変更を保存して（コミットして）」「さっきの状態に戻して」と AI に頼めば操作してくれます。

### 2. AIコーディングエージェントを1つ入れる

どれか1つあれば始められます。**どれにするか迷ったら、[どのエージェントを選ぶか](01-choose-agent.md#どのエージェントを選ぶか)** の選び方の流れを見てください。すでに契約しているサービスがあればそれを、特になければインストールが簡単な **Claude Code** か **Antigravity** がおすすめです。GitHub アカウントがあれば、無料プランのある **GitHub Copilot** も選べます。

#### Claude Code（Anthropic）

公式の手順: [Claude Code のセットアップ](https://code.claude.com/docs/en/setup)

```zsh
curl -fsSL https://claude.ai/install.sh | bash
```

ターミナルを開き直してから起動します。初回はブラウザが開いてログインを求められます。

```zsh
claude
```

#### Antigravity（Google の Gemini）

Antigravity は、[配布ページ](https://antigravity.google/download)で使い方ごとに分かれています。**Orca から使うには CLI 版（Antigravity CLI）が必要**です。

| 配布ページの名前 | どんなときに入れるか |
|---|---|
| **Antigravity CLI** | Orca やターミナルで使う（このガイドの基本） |
| **Antigravity IDE (Standalone)** | エディタでファイルを見ながら使いたい |

1. CLI 版を入れます（配布ページの Antigravity CLI と同じコマンドです）

```zsh
curl -fsSL https://antigravity.google/cli/install.sh | bash
```

2. エディタでも使いたい場合は、配布ページから **Antigravity IDE (Standalone)** の Mac 版をダウンロードしてインストールします
3. ターミナルを開き直してから、次のコマンドで使い方が表示されれば OK です。初回は、Gemini を使っている Google アカウントでログインします

```zsh
agy --help
```

#### Codex（OpenAI）

公式のインストーラーで入れます（公式の手順: [Codex CLI](https://learn.chatgpt.com/docs/codex/cli)）。

```zsh
curl -fsSL https://chatgpt.com/codex/install.sh | sh
```

> Homebrew を入れている場合は `brew install --cask codex` でも入ります。

インストールが終わったら、**表示される案内に従い、ターミナルを閉じて開き直してから**起動します（開き直さないと `command not found` と出ることがあります）。初回は ChatGPT アカウントでのログインを求められます。

```zsh
codex
```

#### GitHub Copilot（GitHub）

GitHub Copilot の CLI 版です。**このガイドでは GitHub アカウントでログインします**（[ステップアップ](#ステップアップ) の手順でアカウントを作っておきます。会社の Microsoft Entra アカウントでログインできる場合もあります: [ログイン方法](https://docs.github.com/ja/copilot/how-tos/copilot-cli/set-up-copilot-cli/authenticate-copilot-cli)）。公式のインストールスクリプトで入れます（公式の手順: [Copilot CLI のインストール](https://docs.github.com/ja/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli)）。

```zsh
curl -fsSL https://gh.io/copilot-install | bash
```

> Homebrew を入れている場合は `brew install --cask copilot-cli` でも入ります。

インストールが終わったら、**表示される案内に従い、ターミナルを閉じて開き直してから**起動します。初回は GitHub アカウントでのログインを求められます（画面の案内に沿って `/login` を実行します）。

```zsh
copilot
```

#### 「command not found」と出たら

多くのエージェントは `~/.local/bin` にインストールされます。次を実行してから、ターミナルを開き直してください。

```zsh
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
```

### 3. Orca を入れる

[onOrca.dev/download](https://onorca.dev/download) から DMG をダウンロードします（Apple Silicon と Intel で分かれています。迷ったら画面左上の Apple メニュー → 「この Mac について」で「チップ」が Apple M〜 なら Apple Silicon）。

使い方は [04-orca.md](04-orca.md) を参照してください。

### 4. 動作確認

```zsh
git --version
```

続けて、入れたエージェントのコマンドを実行します（Claude Code なら `claude --version`、Codex なら `codex --version`、Antigravity なら `agy --help`、GitHub Copilot なら `copilot --version`）。

```zsh
claude --version
```

ここまでできれば、AI 駆動開発を始められます。次は [04-orca.md](04-orca.md) へ進みます。

### 5. 効率化アプリ（任意・おすすめ）

各公式サイトからダウンロードできます（Homebrew があれば `brew install --cask raycast shottr clipy` でも入ります）。

| アプリ | 用途 |
|---|---|
| [Raycast](https://www.raycast.com/) | ランチャー。アプリ起動・クリップボード履歴・ウィンドウ整列・よく使う文章の登録など |
| [Alfred](https://www.alfredapp.com/) | Raycast と役割が近いランチャー。どちらか1つで十分 |
| [Shottr](https://shottr.cc/) | スクリーンショット。注釈・モザイク・画像内の文字のコピー（OCR）ができる |
| [Clipy](https://clipy-app.com/) | 無料のクリップボード履歴。過去にコピーした文章を呼び出せる。よく使う文章（スニペット）も登録できる |

使い方は [07-tools.md](07-tools.md#効率化アプリ) を参照してください。

---

## ステップアップ

### Homebrew（アプリをコマンドで入れる仕組み）

公式サイト: [brew.sh](https://brew.sh/ja/)

```zsh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

完了時に表示される「Next steps」の2行（`echo ... >> ~/.zprofile` と `eval ...`）を必ず実行してください。

```zsh
brew --version
```

### GitHub アカウントと GitHub CLI

GitHub を使うと、作ったものを**ネット上にバックアップ**したり、**他の人と共有・レビュー**したりできます。1人で手元だけで作るなら、なくても困りません。

1. [github.com](https://github.com/) でアカウントを作成
2. GitHub CLI を入れてログイン

```zsh
brew install gh
```

ログインします。質問には `GitHub.com` → `HTTPS` → `Login with a web browser` の順に答えます。

```zsh
gh auth login
```

ログインできたか確認します。

```zsh
gh auth status
```

3. 変更履歴のメールアドレスを、GitHub の非公開用アドレスに変更（公開リポジトリでメールアドレスを出さないため）
   - GitHub の Settings → Emails にある `xxxx+ユーザー名@users.noreply.github.com` を使います

```zsh
git config --global user.email "xxxx+ユーザー名@users.noreply.github.com"
git config --global push.autoSetupRemote true
```

4. 作ったものを GitHub にバックアップします。いちばん簡単なのは、そのプロジェクトでエージェントに頼むことです。

```
このプロジェクトを GitHub の非公開リポジトリにバックアップして
```

自分で行う場合は、プロジェクトのフォルダ（例: `my-app`）に移動して、次を実行します。**非公開（private）**のリポジトリが作られ、これまでの変更がまとめて送られます。

```zsh
cd ~/orca/projects/my-app
gh repo create my-app --private --source=. --remote=origin --push
```

GitHub のページ（`https://github.com/あなたのユーザー名/my-app`）を開いて、自分のファイルが表示されれば完了です。2回目からは、エージェントに「GitHub に送って（push して）」と頼むだけです。

---

## 開発者向け

Web アプリなどを本格的に作るときに使います。

```zsh
brew install mise ripgrep jq
echo 'eval "$(mise activate zsh)"' >> ~/.zshrc
source ~/.zshrc
mise use -g node@lts
node -v
```

| ツール | 説明 |
|---|---|
| `mise` + Node.js | プログラムの実行環境。Web アプリ開発や、npm でツールを入れるときに必要 |
| `ripgrep` (`rg`) | 高速なファイル検索 |
| `jq` | JSON の整形 |

エディタ（AI が書いたコードを確認する用）：

```zsh
brew install --cask visual-studio-code
```

---

## よくあるトラブル

| 症状 | 対処 |
|---|---|
| `command not found` | ターミナルを開き直す。それでもダメなら、基本コース2の「command not found と出たら」を実行 |
| `xcode-select` で「すでにインストールされています」 | Git はもう入っています。そのまま次へ |
| `brew: command not found` | Homebrew の「Next steps」が未実行。`~/.zprofile` を確認 |
| `git push` で認証エラー | `gh auth setup-git` を実行 |
| 何をすればいいかわからない | エラー文をそのまま AI に貼って「どうすればいい？」と聞く |

---

<!-- nav-bottom -->
| ← 前へ: [01 AI エージェントの選び方](01-choose-agent.md) | [目次](../README.md#読む順番) | 次へ: [04 Orca のセットアップ](04-orca.md) → |
|:---|:---:|---:|
<!-- /nav-bottom -->
