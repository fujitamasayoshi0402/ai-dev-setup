# Windows セットアップ

Windows 10 / 11 で AI 駆動開発を始めるための手順です。基本は **PowerShell（管理者不要）** で作業します。ターミナルは「Windows Terminal」を推奨します（Windows 11 は標準搭載）。

## 0. 全体像

| ツール | 用途 | 必須 |
|---|---|---|
| winget | パッケージ管理（Windows 標準） | ✅ |
| Git for Windows | git と Git Bash | ✅ |
| GitHub CLI (`gh`) | PR操作など | ✅ |
| Node.js | ランタイム | ✅ |
| Claude Code | AIコーディングエージェント | ✅ |
| Codex CLI | 別系統のAIエージェント | 任意 |
| Orca | worktree × 複数エージェントの管理 | 推奨（[04-orca.md](04-orca.md)） |
| WSL2 | Linux 環境が必要な場合のみ | 任意 |

## 1. winget の確認

```powershell
winget --version
```

表示されない場合は Microsoft Store で「アプリ インストーラー」を更新してください。

## 2. 基本ツール

```powershell
winget install --id Git.Git -e
winget install --id GitHub.cli -e
winget install --id OpenJS.NodeJS.LTS -e
winget install --id BurntSushi.ripgrep.MSVC -e
winget install --id jqlang.jq -e
```

**インストール後は Windows Terminal を一度閉じて開き直してください**（PATH を反映させるため）。

```powershell
git --version
gh --version
node -v
```

> Node のバージョンを複数使い分けたい場合は `winget install jdx.mise` で mise も使えます。

## 3. Git の初期設定

```powershell
git config --global user.name "あなたの名前"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --global push.autoSetupRemote true
git config --global core.autocrlf input
git config --global core.longpaths true
```

- `core.autocrlf input` … 改行コードを LF に揃え、Mac メンバーとの差分トラブルを防ぎます
- `core.longpaths true` … `node_modules` などの長いパスでのエラーを防ぎます

## 4. GitHub にログイン

```powershell
gh auth login
gh auth setup-git
```

## 5. Claude Code

PowerShell で実行します（管理者権限は不要）。

```powershell
irm https://claude.ai/install.ps1 | iex
```

ターミナルを開き直して確認・ログインします。

```powershell
claude --version
claude
```

### `claude` が見つからない場合

インストール先 `%USERPROFILE%\.local\bin` をユーザー環境変数 PATH に追加します。

```powershell
[Environment]::SetEnvironmentVariable("Path", "$env:USERPROFILE\.local\bin;" + [Environment]::GetEnvironmentVariable("Path", "User"), "User")
```

その後ターミナルを開き直します。

### Git Bash について

Git for Windows を入れておくと、Claude Code が Bash ツールでコマンドを実行できます（入っていない場合は PowerShell で実行）。Mac メンバーとスクリプトを共有しやすいので **Git for Windows の導入を推奨** します。

## 6. Codex CLI（任意）

```powershell
npm install -g @openai/codex
codex
```

## 7. PowerShell の実行ポリシー（エラーが出た場合のみ）

`npm` や `.ps1` の実行で「スクリプトの実行が無効」と出る場合：

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

## 8. WSL2 を使う場合（任意）

Linux 前提のツールチェーン（Docker 開発など）が必要なときのみ。

```powershell
wsl --install
```

再起動後、Ubuntu 内では [Mac 版](01-setup-mac.md) の Homebrew 以外の手順（`curl ... install.sh` など）がほぼそのまま使えます。
**注意:** リポジトリは Windows 側（`C:\`）ではなく WSL 側（`~/`）に clone すると高速です。

## 9. エディタ（任意）

```powershell
winget install --id Microsoft.VisualStudioCode -e
```

## 10. 動作確認チェックリスト

```powershell
git --version; gh auth status; node -v; claude --version
```

すべて通れば完了です。次は [便利ツールの紹介](03-tools.md) → [Orca のセットアップ](04-orca.md) へ。

## よくあるトラブル

| 症状 | 対処 |
|---|---|
| インストールしたコマンドが見つからない | ターミナルを開き直す。それでもダメなら PATH を確認 |
| 「スクリプトの実行が無効」 | 手順7の実行ポリシー変更 |
| 改行コードの差分が大量に出る | `core.autocrlf input` を設定し、リポジトリに `.gitattributes`（`* text=auto eol=lf`）を置く |
| `Filename too long` | `git config --global core.longpaths true` |
| 日本語ユーザー名で動作が不安定 | パスに日本語・空白を含まない作業フォルダ（例: `C:\dev`）を使う |
