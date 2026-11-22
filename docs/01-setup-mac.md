# Mac セットアップ

macOS（Apple Silicon / Intel）で AI 駆動開発を始めるための手順です。**プログラミング経験がなくても進められる**ように、3つのコースに分けています。

| コース | 対象 | 入れるもの |
|---|---|---|
| [基本コース](#基本コース全員) | 全員 | Git ＋ AIエージェント1つ ＋ Orca |
| [ステップアップ](#ステップアップ) | バックアップ・共有もしたい人 | Homebrew、GitHub アカウント、GitHub CLI |
| [開発者向け](#開発者向け) | Web アプリなどを本格的に作る人 | Node.js / mise、ripgrep / jq、エディタ |

まずは **基本コースだけ** で大丈夫です。足りないものが出てきたら、その時点で AI に「〇〇を入れて」と頼めば入れてくれます。

## ターミナルの開き方

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

どれか1つあれば始められます。違いは [03-tools.md](03-tools.md#aiコーディングエージェント) を参照してください。迷ったら、インストールが簡単な **Claude Code** か **Antigravity** がおすすめです。

#### Claude Code（Anthropic）

```zsh
curl -fsSL https://claude.ai/install.sh | bash
```

ターミナルを開き直してから起動します。初回はブラウザが開いてログインを求められます。

```zsh
claude
```

#### Antigravity（Google）

[antigravity.google/download](https://antigravity.google/download) から Mac 版をダウンロードしてインストールします。エディタ一体型のアプリで、Google アカウントでログインします。

ターミナルで使う CLI 版（`agy`）もあります。入れ方は [公式のインストール手順](https://antigravity.google/docs/cli/install) を参照してください。

#### Codex（OpenAI）

Codex の CLI 版は、Homebrew または Node.js（npm）を使って入れます。先に [ステップアップ](#ステップアップ) の Homebrew を入れてから、次を実行します。

```zsh
brew install --cask codex
codex           # 初回起動で ChatGPT アカウントにログイン
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
claude --version    # 入れたエージェントに合わせて（codex --version / agy --help）
```

ここまでできれば、AI 駆動開発を始められます。次は [04-orca.md](04-orca.md) へ進みます。

### 5. 効率化アプリ（任意・おすすめ）

各公式サイトからダウンロードできます（Homebrew があれば `brew install --cask raycast shottr` でも入ります）。

| アプリ | 用途 |
|---|---|
| [Raycast](https://www.raycast.com/) | ランチャー。アプリ起動・クリップボード履歴・ウィンドウ整列・よく使う文章の登録など |
| [Alfred](https://www.alfredapp.com/) | Raycast と役割が近いランチャー。どちらか1つで十分 |
| [Shottr](https://shottr.cc/) | スクリーンショット。注釈・モザイク・画像内の文字のコピー（OCR）ができる |

使い方は [03-tools.md](03-tools.md#効率化アプリ) を参照してください。

---

## ステップアップ

### Homebrew（アプリをコマンドで入れる仕組み）

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
gh auth login       # GitHub.com → HTTPS → Login with a web browser の順に選ぶ
gh auth status
```

3. 変更履歴のメールアドレスを、GitHub の非公開用アドレスに変更（公開リポジトリでメールアドレスを出さないため）
   - GitHub の Settings → Emails にある `xxxx+ユーザー名@users.noreply.github.com` を使います

```zsh
git config --global user.email "xxxx+ユーザー名@users.noreply.github.com"
git config --global push.autoSetupRemote true
```

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
