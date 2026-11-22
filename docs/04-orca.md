# Orca のセットアップ

[Orca](https://github.com/stablyai/orca) は、**Git worktree を単位に複数の AI エージェントを並行で動かす**ためのデスクトップアプリです。
「1タスク = 1 worktree = 1 エージェント」で作業を分けるので、エージェント同士がファイルを取り合わず、並行作業が安全にできます。

## なぜ Orca を使うのか

| 課題 | Orca での解決 |
|---|---|
| 複数タスクを同時に AI に頼むと変更が混ざる | タスクごとに worktree（別フォルダ・別ブランチ）を自動作成 |
| ターミナルが大量に開いてどれが何かわからない | worktree ごとにタブ・ペインを整理して表示 |
| エージェントの状態（作業中/入力待ち）がわからない | サイドバーで状態を一覧表示・通知 |
| 差分確認や PR 作成が面倒 | アプリ内でファイル差分・Source Control を確認 |

Claude Code・Codex など、ターミナルで動く CLI エージェントなら基本的に何でも使えます。

## 1. インストール

### Mac

```zsh
brew install --cask stablyai/orca/orca
```

または [onOrca.dev/download](https://onorca.dev/download) / [GitHub Releases](https://github.com/stablyai/orca/releases) から DMG（Apple Silicon / Intel）をダウンロード。

### Windows

[GitHub Releases](https://github.com/stablyai/orca/releases) から `.exe` セットアップをダウンロードして実行。

> Windows 向けの修正が RC（リリース候補）版にだけ入っている時期があります。安定版で不具合がある場合は Releases ページで最新の RC を確認してください。

## 2. 初期設定

1. Orca を起動
2. 使うエージェント（Claude / Codex など）がインストール済みであることを確認
   ```zsh
   orca agent installed
   ```
   `orca` コマンドが使えない場合は、Orca の設定から CLI をインストールしてください。
3. Orca のスキルを各エージェントに入れておくと、エージェント自身が Orca を操作できるようになります
   ```zsh
   orca skills install --all
   ```

## 3. リポジトリを登録する

GUI のサイドバーから「Add project」でクローン済みフォルダを選ぶか、リポジトリ URL からクローンします。CLI の場合：

```zsh
orca repo add --path ~/orca/projects/my-app
orca repo list
```

## 4. worktree を作ってエージェントを起動

GUI ではプロジェクトの「＋」から新しい workspace（worktree）を作成し、エージェントを選びます。CLI の場合：

```zsh
# worktree を作成し、そのまま Claude に指示を渡して起動
orca worktree create --repo name:my-app --name login-page \
  --agent claude --prompt "ログイン画面を作って"

# 既存の worktree でエージェントを追加起動
orca agent launch --agent codex --worktree name:login-page \
  --prompt "この変更をレビューして"
```

GitHub Issue と紐づける場合は `--issue 12` を付けると、Issue の情報を参照しながら作業できます。

## 5. 基本の作業フロー

```
main ─┬─ worktree: feature-a  ← Claude が実装
      ├─ worktree: feature-b  ← Claude が別機能を実装
      └─ worktree: review-a   ← Codex が feature-a をレビュー
```

1. タスクごとに worktree を作る（名前は `feature-xxx` / `fix-xxx` など）
2. エージェントに指示 → 他の worktree で別タスクを並行
3. サイドバーで「入力待ち」になったものから順に確認・返答
4. 差分を確認して PR 作成（エージェントに `gh pr create` を頼んでもOK）
5. マージしたら worktree を削除
   ```zsh
   orca worktree rm name:feature-a
   ```

## 6. よく使う CLI

| コマンド | 用途 |
|---|---|
| `orca status` | Orca の起動状態確認 |
| `orca worktree list` | worktree 一覧 |
| `orca worktree ps` | 各 worktree の状況サマリー |
| `orca agent list` | 起動中のエージェント一覧 |
| `orca terminal read` | ターミナル出力の読み取り |
| `orca automations create` | 定期実行タスクの作成 |

全コマンドは `orca --help`、エージェント向けのスキーマは `orca agent-context` で確認できます。

## 7. Tips

- **並行数は 2〜3 から**。人間のレビューが追いつく数に抑えるのがコツです
- **セットアップスクリプト**を設定しておくと、worktree 作成時に `npm install` や `.env` のコピーを自動実行できます
- `.env` は worktree に自動でコピーされないことがあるので、上記スクリプトで対応するのがおすすめ
- 実装は Claude、レビューは Codex のように**別モデルで相互チェック**すると品質が上がります
