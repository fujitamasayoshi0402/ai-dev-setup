# 便利ツールの紹介と使い方

## Claude Code

ターミナルで動く AI コーディングエージェント。ファイルの読み書き・コマンド実行・Git 操作まで任せられます。

### 基本操作

```zsh
cd your-repo
claude                    # 対話モードで起動
claude "READMEを要約して"   # 最初の指示を渡して起動
claude -c                 # 直前の会話を再開
claude -r                 # 過去の会話を選んで再開
```

### よく使うスラッシュコマンド

| コマンド | 用途 |
|---|---|
| `/init` | リポジトリを解析して `CLAUDE.md` を自動生成 |
| `/clear` | 会話をリセット（タスクが変わったら使う） |
| `/compact` | 会話を要約して文脈を節約 |
| `/model` | モデル切り替え |
| `/config` | 設定画面 |
| `/permissions` | 許可するコマンドの管理 |
| `/mcp` | MCP サーバー（外部連携）の管理 |
| `/code-review` | 変更のレビュー |
| `/doctor` | インストール状態の診断 |

### キー操作

| キー | 用途 |
|---|---|
| `Shift + Tab` | モード切替（通常 / 自動承認 / Plan モード） |
| `Esc` | 実行中の処理を止める |
| `Esc` 2回 | 過去のメッセージに戻って編集 |
| `@ファイル名` | ファイルを文脈に添付 |
| `!コマンド` | シェルコマンドを直接実行 |

### CLAUDE.md（最重要）

プロジェクトのルートに置くと、毎回自動で読み込まれる「AIへの常設指示書」です。

```markdown
# プロジェクト概要
- Next.js + TypeScript の Web アプリ

# コマンド
- 開発サーバー: `npm run dev`
- テスト: `npm test`
- Lint: `npm run lint`

# ルール
- コミット前に必ず lint と test を通す
- 新しいライブラリを追加する前に確認する
```

| 置き場所 | 効く範囲 |
|---|---|
| `~/.claude/CLAUDE.md` | 自分の全プロジェクト（例: 「日本語で回答」） |
| `<repo>/CLAUDE.md` | そのリポジトリ（チームで共有・コミットする） |
| `<repo>/CLAUDE.local.md` | 自分だけのローカル設定（`.gitignore` に入れる） |

### 拡張機能

| 機能 | 概要 | 置き場所 |
|---|---|---|
| Skills | 手順書をパッケージ化し、必要な時に自動で読み込ませる | `.claude/skills/<名前>/SKILL.md` |
| Hooks | 「編集後に自動で formatter を走らせる」などの自動処理 | `.claude/settings.json` |
| MCP | GitHub・Jira・ブラウザなど外部サービスとの連携 | `claude mcp add ...` |
| Subagents | 調査・レビューなど役割別のエージェント | `.claude/agents/*.md` |

## Codex CLI

OpenAI 製のコーディングエージェント。Claude Code と同様に使えます。**別のモデルにレビューさせる「セカンドオピニオン」役**として併用すると効果的です。

```zsh
codex                      # 対話モード
codex exec "テストを追加して"  # 非対話で実行
```

プロジェクト指示は `AGENTS.md` に書きます。Claude Code と共有したい場合は `CLAUDE.md` から `@AGENTS.md` で参照するとまとめられます。

## GitHub CLI（gh）

```zsh
gh repo create my-app --private --clone   # リポジトリ作成
gh pr create --fill                       # PR作成
gh pr list                                # PR一覧
gh pr checkout 12                         # PRをローカルに取得
gh issue list                             # Issue一覧
gh run watch                              # CI の進捗を見る
```

AIエージェントも `gh` を使って PR 作成やCI確認ができるので、ログインしておくと自動化の幅が広がります。

## mise

プロジェクトごとにランタイムのバージョンを固定します。

```zsh
mise use node@22          # このディレクトリで Node 22 を使う（mise.toml が作られる）
mise install              # mise.toml に書かれたバージョンを一括インストール
```

`mise.toml` をコミットしておけば、メンバー全員（とAI）が同じバージョンで動かせます。

## ripgrep / jq

```zsh
rg "useState" src/        # 高速なコード検索
cat data.json | jq '.items[0]'
```

## その他おすすめ

| ツール | 用途 |
|---|---|
| VS Code / Cursor | AI が書いた差分の確認・手直し |
| Raycast（Mac） / PowerToys（Win） | ランチャー・ショートカット |
| Docker Desktop | DB などのミドルウェアをローカルで動かす |
