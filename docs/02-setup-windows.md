# Windows セットアップ

Windows 10 / 11 で AI 駆動開発を始めるための手順です。**プログラミング経験がなくても進められる**ように、3つのコースに分けています。

| コース | 対象 | 入れるもの |
|---|---|---|
| [基本コース](#基本コース全員) | 全員 | Git for Windows ＋ AIエージェント1つ ＋ Orca |
| [ステップアップ](#ステップアップ) | バックアップ・共有もしたい人 | GitHub アカウント、GitHub CLI |
| [開発者向け](#開発者向け) | Web アプリなどを本格的に作る人 | Node.js、ripgrep / jq、WSL2、エディタ |

まずは **基本コースだけ** で大丈夫です。足りないものが出てきたら、その時点で AI に「〇〇を入れて」と頼めば入れてくれます。

## ターミナルの開き方

スタートメニューで「ターミナル」（Windows Terminal）と検索して開きます。Windows 10 で見つからない場合は、Microsoft Store から「Windows Terminal」を入れてください。
以降のコマンドは、ここ（PowerShell）にコピー＆ペーストして Enter で実行します。**管理者として実行する必要はありません**（WSL2 の手順だけは例外です）。

---

## 基本コース（全員）

### 1. Git for Windows を入れる

Git は「ファイルの変更履歴を残す仕組み」です。**AI が変更を壊しても元に戻せる、取り消しボタン**の役割をします。Orca が1タスクごとに作業フォルダを分ける機能（worktree）も Git を使っています。また、AIエージェントの多くは Git for Windows に入っている「Git Bash」を使ってコマンドを実行します。

```powershell
winget install --id Git.Git -e
```

> `winget` が使えない場合は、[git-scm.com](https://git-scm.com/downloads/win) からインストーラーをダウンロードし、設定はすべて初期値のまま「Next」で進めます。

**インストールしたら、ターミナルを一度閉じて開き直してください。**

```powershell
git --version
```

続けて、変更履歴に残す名前などを設定します（メールアドレスは、後で GitHub を使うときに変更できます）。

```powershell
git config --global user.name "あなたの名前"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --global core.autocrlf input
git config --global core.longpaths true
```

- `core.autocrlf input` … 改行コードを揃えて、Mac のメンバーとのトラブルを防ぎます
- `core.longpaths true` … 長いファイル名でのエラーを防ぎます

> Git のコマンドを覚える必要はありません。「ここまでの変更を保存して（コミットして）」「さっきの状態に戻して」と AI に頼めば操作してくれます。

### 2. AIコーディングエージェントを1つ入れる

どれか1つあれば始められます。違いは [03-tools.md](03-tools.md#aiコーディングエージェント) を参照してください。迷ったら、インストールが簡単な **Claude Code** か **Antigravity** がおすすめです。

#### Claude Code（Anthropic）

公式の手順: [Claude Code のセットアップ](https://code.claude.com/docs/en/setup)

```powershell
irm https://claude.ai/install.ps1 | iex
```

ターミナルを開き直してから起動します。初回はブラウザが開いてログインを求められます。

```powershell
claude
```

#### Antigravity（Google）

Antigravity には、エディタ一体型のアプリ版と、ターミナルで使う CLI 版（`agy`）があります。**Orca から使うには CLI 版が必要**です。

1. アプリ版: [antigravity.google/download](https://antigravity.google/download) から Windows 版をダウンロードしてインストールし、Google アカウントでログインします
2. CLI 版: [公式の Get started](https://antigravity.google/docs/getting-started) にある CLI のインストール手順に従って入れます
3. ターミナルを開き直してから、次のコマンドで使い方が表示されれば OK です

```powershell
agy --help
```

#### Codex（OpenAI）

Codex の CLI 版は Node.js（npm）を使って入れます（公式の手順: [Codex CLI](https://developers.openai.com/codex/cli)）。先に [開発者向け](#開発者向け) の Node.js を入れてから、次を実行します。

```powershell
npm install -g @openai/codex
codex
```

#### 「認識されません」と出たら

ターミナルを開き直しても見つからない場合は、インストール先を PATH に追加します。Claude Code の例です（インストール先はエージェントによって違います）。

```powershell
[Environment]::SetEnvironmentVariable("Path", "$env:USERPROFILE\.local\bin;" + [Environment]::GetEnvironmentVariable("Path", "User"), "User")
```

- 実行したら、**ターミナルを閉じて開き直して**ください
- このコマンドは**1回だけ**実行します（くり返すと、同じ場所が何度も登録されてしまいます）

### 3. Orca を入れる

[onOrca.dev/download](https://onorca.dev/download) または [GitHub Releases](https://github.com/stablyai/orca/releases) から Windows 版（`.exe`）をダウンロードして実行します。

> Windows 向けの修正が RC（リリース候補）版にだけ入っている時期があります。うまく動かない場合は、Releases ページで最新の RC 版を試してください。

使い方は [04-orca.md](04-orca.md) を参照してください。

### 4. 動作確認

```powershell
git --version
```

続けて、入れたエージェントのコマンドを実行します（Claude Code なら `claude --version`、Codex なら `codex --version`、Antigravity なら `agy --help`）。

```powershell
claude --version
```

ここまでできれば、AI 駆動開発を始められます。次は [04-orca.md](04-orca.md) へ進みます（エージェントの違いを先に知りたい場合は [03-tools.md](03-tools.md)）。

### 5. 効率化アプリ（任意・おすすめ）

| アプリ | 用途 |
|---|---|
| PowerToys（`winget install --id Microsoft.PowerToys -e`） | ランチャー（`Alt + Space`）、ウィンドウ整列、画面上の文字のコピー（OCR）など |
| Snipping Tool（標準） | スクリーンショット（`Win + Shift + S`） |
| クリップボード履歴（標準） | `Win + V`。最初に一度オンにしておく |

---

## ステップアップ

### GitHub アカウントと GitHub CLI

GitHub を使うと、作ったものを**ネット上にバックアップ**したり、**他の人と共有・レビュー**したりできます。1人で手元だけで作るなら、なくても困りません。

1. [github.com](https://github.com/) でアカウントを作成
2. GitHub CLI を入れます

```powershell
winget install --id GitHub.cli -e
```

**ターミナルを閉じて開き直してから**、ログインします。質問には `GitHub.com` → `HTTPS` → `Login with a web browser` の順に答えます。

```powershell
gh auth login
gh auth setup-git
```

3. 変更履歴のメールアドレスを、GitHub の非公開用アドレスに変更（公開リポジトリでメールアドレスを出さないため）
   - GitHub の Settings → Emails にある `xxxx+ユーザー名@users.noreply.github.com` を使います

```powershell
git config --global user.email "xxxx+ユーザー名@users.noreply.github.com"
git config --global push.autoSetupRemote true
```

---

## 開発者向け

Web アプリなどを本格的に作るときに使います。入れたらターミナルを開き直してください。

```powershell
winget install --id OpenJS.NodeJS.LTS -e
winget install --id BurntSushi.ripgrep.MSVC -e
winget install --id jqlang.jq -e
winget install --id Microsoft.VisualStudioCode -e
```

| ツール | 説明 |
|---|---|
| Node.js | プログラムの実行環境。Web アプリ開発や、npm でツールを入れるときに必要 |
| ripgrep (`rg`) | 高速なファイル検索 |
| jq | JSON の整形 |
| VS Code | AI が書いたコードを確認するエディタ |

> Node のバージョンを複数使い分けたい場合は、`winget install --id jdx.mise -e` で mise も使えます。

### PowerShell の実行ポリシー（エラーが出た場合のみ）

`npm` などを実行して「このシステムではスクリプトの実行が無効になっている」と出る場合：

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

### WSL2（Linux 環境が必要な場合のみ）

Docker を使う開発など、Linux 前提のツールが必要なときだけ使います。

**この手順だけは管理者権限が必要です。** スタートメニューで「ターミナル」を右クリック →「管理者として実行」で開いてから実行し、終わったらパソコンを再起動します（公式の手順: [WSL のインストール](https://learn.microsoft.com/ja-jp/windows/wsl/install)）。

```powershell
wsl --install
```

再起動後の Ubuntu の中では、[Mac 版](01-setup-mac.md) の手順のうち Homebrew 以外（`curl ... | bash` 形式のインストールなど）がほぼそのまま使えます。リポジトリは Windows 側（`C:\`）ではなく WSL 側（`~/`）に置くと高速です。

---

## よくあるトラブル

| 症状 | 対処 |
|---|---|
| 「〇〇 は認識されません」 | ターミナルを開き直す。それでもダメなら PATH を確認 |
| 「スクリプトの実行が無効」 | 開発者向けの「実行ポリシー」を実行 |
| 改行コードの違いで大量の差分が出る | `core.autocrlf input` を設定し、リポジトリに `.gitattributes`（`* text=auto eol=lf`）を置く |
| `Filename too long` | `git config --global core.longpaths true` |
| ユーザー名が日本語で動作が不安定 | 日本語や空白を含まない作業フォルダ（例: `C:\dev`）を使う |
| 何をすればいいかわからない | エラー文をそのまま AI に貼って「どうすればいい？」と聞く |
