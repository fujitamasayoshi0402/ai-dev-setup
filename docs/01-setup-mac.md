# Mac セットアップ

macOS（Apple Silicon / Intel）で AI 駆動開発を始めるための手順です。ターミナルは標準の「ターミナル.app」で構いません（シェルは zsh 前提）。

## 0. 全体像

| ツール | 用途 | 必須 |
|---|---|---|
| Xcode Command Line Tools | git などの基本ツール | ✅ |
| Homebrew | パッケージ管理 | ✅ |
| Git / GitHub CLI (`gh`) | バージョン管理・PR操作 | ✅ |
| mise + Node.js | ランタイム管理 | ✅ |
| Claude Code | AIコーディングエージェント | ✅ |
| Codex CLI | 別系統のAIエージェント（レビュー役など） | 任意 |
| Orca | worktree × 複数エージェントの管理 | 推奨（[04-orca.md](04-orca.md)） |

## 1. Xcode Command Line Tools

```zsh
xcode-select --install
```

ダイアログが出たら「インストール」。完了後に確認します。

```zsh
git --version
```

## 2. Homebrew

```zsh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

インストール完了時に表示される「Next steps」の2行（`echo ... >> ~/.zprofile` と `eval ...`）を必ず実行してください。

```zsh
brew --version
```

## 3. 基本ツール

```zsh
brew install git gh mise ripgrep jq
```

| ツール | 説明 |
|---|---|
| `gh` | GitHub CLI。PR作成・Issue確認をターミナルから |
| `mise` | Node / Python などのバージョン管理 |
| `ripgrep` (`rg`) | 高速検索。AIエージェントも内部で利用 |
| `jq` | JSON 整形 |

## 4. Git の初期設定

```zsh
git config --global user.name "あなたの名前"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --global push.autoSetupRemote true
```

## 5. GitHub にログイン

```zsh
gh auth login
```

`GitHub.com` → `HTTPS` → `Login with a web browser` を選択。これで `git push` の認証も通ります。

```zsh
gh auth status
```

## 6. mise で Node.js を入れる

```zsh
echo 'eval "$(mise activate zsh)"' >> ~/.zshrc
source ~/.zshrc
mise use -g node@lts
node -v
```

## 7. Claude Code

```zsh
curl -fsSL https://claude.ai/install.sh | bash
```

新しいターミナルを開いて確認・ログインします。

```zsh
claude --version
claude          # 初回起動でブラウザが開きログイン
```

`claude: command not found` の場合は `~/.local/bin` が PATH に入っていません。

```zsh
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

診断は `claude doctor` で行えます。

## 8. Codex CLI（任意）

```zsh
npm install -g @openai/codex
codex          # 初回起動で ChatGPT アカウントにログイン
```

## 9. エディタ（任意）

AIエージェントが書いたコードの確認用に、好みのエディタを入れておきます。

```zsh
brew install --cask visual-studio-code
```

## 10. 動作確認チェックリスト

```zsh
git --version && gh auth status && node -v && claude --version
```

すべて通れば完了です。次は [便利ツールの紹介](03-tools.md) → [Orca のセットアップ](04-orca.md) へ。

## よくあるトラブル

| 症状 | 対処 |
|---|---|
| `brew: command not found` | Homebrew の「Next steps」を実行していない。`~/.zprofile` を確認 |
| `claude` が見つからない | `~/.local/bin` を PATH に追加してターミナルを開き直す |
| `git push` で認証エラー | `gh auth setup-git` を実行 |
| Node のバージョンが切り替わらない | `~/.zshrc` に `mise activate` が入っているか確認 |
