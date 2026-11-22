# Mac セットアップ

macOS（Apple Silicon / Intel）で AI 駆動開発を始めるための手順です。ターミナルは標準の「ターミナル.app」で構いません（シェルは zsh 前提）。

## 0. 全体像

| ツール | 用途 | 必須 |
|---|---|---|
| Xcode Command Line Tools | git などの基本ツール | ✅ |
| Homebrew | パッケージ管理 | ✅ |
| Git / GitHub CLI (`gh`) | バージョン管理・PR操作 | ✅ |
| mise + Node.js | ランタイム管理 | ✅ |
| AIコーディングエージェント | Claude Code / Codex / Antigravity など | ✅（1つ以上） |
| Orca | worktree × 複数エージェントの管理 | 推奨（[04-orca.md](04-orca.md)） |
| 効率化アプリ | Raycast / Alfred / Shottr | 任意 |

## 1. Xcode Command Line Tools

```zsh
xcode-select --install
```

ダイアログが出たら「インストール」を押します。完了したら確認します。

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
| `gh` | GitHub CLI。PR作成・Issue確認をターミナルから行える |
| `mise` | Node / Python などのバージョン管理 |
| `ripgrep` (`rg`) | 高速検索。多くのAIエージェントが内部で利用 |
| `jq` | JSON 整形 |

## 4. Git の初期設定

```zsh
git config --global user.name "あなたの名前"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --global push.autoSetupRemote true
```

> 公開リポジトリでメールアドレスを出したくない場合は、GitHub の Settings → Emails にある `xxxx+ユーザー名@users.noreply.github.com` を使うと安心です。

## 5. GitHub にログイン

```zsh
gh auth login
```

`GitHub.com` → `HTTPS` → `Login with a web browser` の順に選びます。これで `git push` の認証も通ります。

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

## 7. AIコーディングエージェント（1つ以上）

どれか1つあれば始められます。複数入れておくと、「実装はA、レビューはB」のような使い分けができます。違いは [03-tools.md](03-tools.md#aiコーディングエージェント) を参照してください。

### Claude Code（Anthropic）

```zsh
curl -fsSL https://claude.ai/install.sh | bash
claude          # 初回起動でブラウザが開きログイン
```

### Codex CLI（OpenAI）

```zsh
npm install -g @openai/codex
codex           # 初回起動で ChatGPT アカウントにログイン
```

### Antigravity（Google）

エディタ一体型のアプリと、ターミナル用の CLI（`agy`）があります。

```zsh
brew install --cask antigravity     # アプリ版
```

CLI 版は [公式のインストール手順](https://antigravity.google/docs/cli/install) に従って入れます。

```zsh
agy             # 初回起動で Google アカウントにログイン
```

### コマンドが見つからない場合

多くのエージェントは `~/.local/bin` にインストールされます。

```zsh
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

## 8. エディタ（任意）

AIエージェントが書いたコードを確認するために、好みのエディタを入れておきます（Antigravity のアプリ版はエディタも兼ねています）。

```zsh
brew install --cask visual-studio-code
```

## 9. 効率化アプリ（任意・おすすめ）

```zsh
brew install --cask raycast shottr
# Alfred 派の場合: brew install --cask alfred
```

| アプリ | 用途 |
|---|---|
| Raycast | ランチャー。アプリ起動・クリップボード履歴・ウィンドウ整列・スニペットなど |
| Alfred | ランチャー。Raycast と役割が近いので、どちらか1つで十分 |
| Shottr | スクリーンショット。範囲指定・注釈・OCR（画像内の文字のコピー）ができる |

使い方は [03-tools.md](03-tools.md#効率化アプリ) を参照してください。

## 10. 動作確認チェックリスト

```zsh
git --version && gh auth status && node -v
# 入れたエージェントに合わせて
claude --version   # / codex --version / agy --help
```

すべて通れば完了です。次は [便利ツールの紹介](03-tools.md) → [Orca のセットアップ](04-orca.md) へ進みます。

## よくあるトラブル

| 症状 | 対処 |
|---|---|
| `brew: command not found` | Homebrew の「Next steps」が未実行。`~/.zprofile` を確認 |
| エージェントのコマンドが見つからない | `~/.local/bin` を PATH に追加して、ターミナルを開き直す |
| `git push` で認証エラー | `gh auth setup-git` を実行 |
| Node のバージョンが切り替わらない | `~/.zshrc` に `mise activate` が入っているか確認 |
