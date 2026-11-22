# ディレクトリ管理と Tips

## 1. 推奨ディレクトリ構成

「元のリポジトリ（projects）」と「作業用 worktree（workspaces）」を分けて置きます。

```
~/orca/
├── projects/                 # 各リポジトリの本体（main ブランチ）
│   ├── my-app/
│   └── tutorial-documento/
└── workspaces/               # worktree（タスクごとの作業フォルダ）
    ├── my-app/
    │   ├── login-page/       # feature ブランチ
    │   └── fix-header/
    └── tutorial-documento/
        └── docs-setup/
```

| ルール | 理由 |
|---|---|
| `projects/` の本体では直接作業しない | main を常にきれいに保ち、新しい worktree の起点にする |
| 1タスク = 1 worktree | エージェント同士の変更が混ざらない |
| マージしたら worktree を削除 | フォルダとブランチが溜まり続けるのを防ぐ |
| パスに空白を入れない（特に Windows） | ツールによってはエラーになる |

> Windows の場合は `C:\dev\orca\projects\...` のように短いパスを推奨します。

Orca を使わずに手動で worktree を作る場合：

```zsh
cd ~/orca/projects/my-app
git worktree add ../../workspaces/my-app/login-page -b feature/login-page
git worktree list
git worktree remove ../../workspaces/my-app/login-page
```

## 2. リポジトリ内の構成例

```
my-app/
├── CLAUDE.md          # AI への常設指示（コミットする）
├── AGENTS.md          # Codex 等向けの指示（共通化してもOK）
├── .claude/
│   ├── settings.json  # 許可コマンド・hooks（チーム共有）
│   ├── skills/        # プロジェクト固有のスキル
│   └── agents/        # サブエージェント定義
├── docs/              # 仕様・設計メモ（AI にも読ませる）
├── .gitignore         # CLAUDE.local.md, .env, settings.local.json を除外
└── src/
```

`.gitignore` に入れておくもの：

```gitignore
.env
.env.*
CLAUDE.local.md
.claude/settings.local.json
```

## 3. ブランチ・コミット運用

- ブランチ名: `feature/xxx` / `fix/xxx` / `docs/xxx`
- コミットは小さく。AI には「1つの変更ごとにコミットして」と指示する
- main への直接 push は禁止し、必ず PR 経由にする（GitHub のブランチ保護を設定）

## 4. AI への指示のコツ

### 良い指示の型

```
【目的】ログイン画面を作りたい
【条件】既存の Button コンポーネントを使う / メール+パスワード
【完了条件】npm test が通る / 画面で動作確認できる
【やらないこと】API 側の実装はしない
```

### Tips 集

| Tips | 内容 |
|---|---|
| まず計画させる | 大きな作業は Plan モード（`Shift+Tab`）で計画→確認→実装 |
| 完了条件を明示する | 「テストが通るまで」「lint エラー0まで」と書くと自走してくれる |
| タスクが変わったら `/clear` | 古い文脈が残ると精度が落ちる |
| 繰り返し伝えることは CLAUDE.md へ | 毎回言っていることはルール化する |
| 手順化できる作業は Skill へ | リリース手順・レビュー観点などを再利用 |
| 差分は必ず人が読む | AI の変更をそのままマージしない |
| 別モデルでレビュー | Claude が書いて Codex がレビュー（またはその逆） |
| 秘密情報を貼らない | API キーは `.env` に置き、プロンプトに直接書かない |

## 5. 権限設定の考え方

`.claude/settings.json` でよく使う安全なコマンドを許可しておくと、確認ダイアログが減って作業が速くなります。

```json
{
  "permissions": {
    "allow": [
      "Bash(npm run lint)",
      "Bash(npm test)",
      "Bash(git status)",
      "Bash(git diff:*)"
    ],
    "deny": [
      "Read(./.env)",
      "Bash(git push --force:*)"
    ]
  }
}
```

- `rm -rf` や `git push --force` など**取り返しのつかない操作は deny** に
- 全許可モードは worktree 内など影響範囲が限定された場所でのみ使う
