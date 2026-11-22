# Windows セットアップ

Windows 10 / 11 で AI 駆動開発を始めるための手順です。基本は **PowerShell（管理者権限は不要）** で作業します。ターミナルは「Windows Terminal」がおすすめです（Windows 11 には標準で入っています）。

## 0. 全体像

| ツール | 用途 | 必須 |
|---|---|---|
| winget | パッケージ管理（Windows 標準） | ✅ |
| Git for Windows | git と Git Bash | ✅ |
| GitHub CLI (`gh`) | PR操作など | ✅ |
| Node.js | ランタイム | ✅ |
| AIコーディングエージェント | Claude Code / Codex / Antigravity など | ✅（1つ以上） |
| Orca | worktree × 複数エージェントの管理 | 推奨（[04-orca.md](04-orca.md)） |
| WSL2 | Linux 環境が必要な場合のみ | 任意 |

## 1. winget の確認

```powershell
winget --version
```

表示されない場合は、Microsoft Store で「アプリ インストーラー」を更新してください。

## 2. 基本ツール

```powershell
winget install --id Git.Git -e
winget install --id GitHub.cli -e
winget install --id OpenJS.NodeJS.LTS -e
winget install --id BurntSushi.ripgrep.MSVC -e
winget install --id jqlang.jq -e
```

**インストールしたら Windows Terminal を一度閉じて開き直してください**（PATH を反映させるため）。

```powershell
git --version
gh --version
node -v
```

> Node のバージョンを複数使い分けたい場合は、`winget install jdx.mise` で mise も使えます。

## 3. Git の初期設定

```powershell
git config --global user.name "あなたの名前"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --global push.autoSetupRemote true
git config --global core.autocrlf input
git config --global core.longpaths true
```

- `core.autocrlf input` … 改行コードを LF に揃えて、Mac のメンバーとの差分トラブルを防ぎます
- `core.longpaths true` … `node_modules` などの長いパスで起きるエラーを防ぎます

## 4. GitHub にログイン

```powershell
gh auth login
gh auth setup-git
```

## 5. AIコーディングエージェント（1つ以上）

どれか1つあれば始められます。違いは [03-tools.md](03-tools.md#aiコーディングエージェント) を参照してください。

### Claude Code（Anthropic）

```powershell
irm https://claude.ai/install.ps1 | iex
claude
```

### Codex CLI（OpenAI）

```powershell
npm install -g @openai/codex
codex
```

### Antigravity（Google）

- アプリ版: [antigravity.google/download](https://antigravity.google/download) からインストーラーをダウンロードします
- CLI 版（`agy`）: [公式のインストール手順](https://antigravity.google/docs/cli/install) に従います

### コマンドが見つからない場合

ターミナルを開き直しても見つからない場合は、インストール先をユーザー環境変数 PATH に追加します。Claude Code の例です（インストール先はエージェントによって異なります）。

```powershell
[Environment]::SetEnvironmentVariable("Path", "$env:USERPROFILE\.local\bin;" + [Environment]::GetEnvironmentVariable("Path", "User"), "User")
```

### Git Bash について

多くのエージェントは、Git for Windows の Git Bash があると Bash でコマンドを実行できます。Mac のメンバーとスクリプトを共有しやすくなるので、**Git for Windows は必ず入れておく** ことをおすすめします。

## 6. PowerShell の実行ポリシー（エラーが出た場合のみ）

`npm` や `.ps1` を実行して「スクリプトの実行が無効」と出る場合：

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

## 7. WSL2 を使う場合（任意）

Linux 前提のツールチェーン（Docker を使う開発など）が必要なときだけ使います。

```powershell
wsl --install
```

再起動後の Ubuntu の中では、[Mac 版](01-setup-mac.md) の手順のうち Homebrew 以外（`curl ... | bash` 形式のインストールなど）がほぼそのまま使えます。
**注意:** リポジトリは Windows 側（`C:\`）ではなく WSL 側（`~/`）に clone すると高速です。

## 8. エディタ・効率化アプリ（任意）

```powershell
winget install --id Microsoft.VisualStudioCode -e
winget install --id Microsoft.PowerToys -e
```

| アプリ | 用途 |
|---|---|
| PowerToys | ランチャー（`Alt + Space`）、ウィンドウ整列、画面上の文字認識（OCR）など |
| Snipping Tool（標準） | スクリーンショット（`Win + Shift + S`）。画像内の文字もコピーできる |
| クリップボード履歴（標準） | `Win + V`。最初に一度オンにしておく |

## 9. 動作確認チェックリスト

```powershell
git --version; gh auth status; node -v
# 入れたエージェントに合わせて
claude --version   # / codex --version / agy --help
```

すべて通れば完了です。次は [便利ツールの紹介](03-tools.md) → [Orca のセットアップ](04-orca.md) へ進みます。

## よくあるトラブル

| 症状 | 対処 |
|---|---|
| インストールしたコマンドが見つからない | ターミナルを開き直す。それでもダメなら PATH を確認 |
| 「スクリプトの実行が無効」 | 手順6の実行ポリシー変更 |
| 改行コードの差分が大量に出る | `core.autocrlf input` を設定し、リポジトリに `.gitattributes`（`* text=auto eol=lf`）を置く |
| `Filename too long` | `git config --global core.longpaths true` |
| ユーザー名が日本語で動作が不安定 | 日本語や空白を含まないパスの作業フォルダ（例: `C:\dev`）を使う |
