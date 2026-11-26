<!-- nav-top -->
[目次](../README.md#読む順番) ＞ ③ 作る ＞ **09 アプリ・サイトを作る**
<!-- /nav-top -->

# アプリ・サイトを作る（フォルダの管理）

アプリや Web サイトなど、**プログラムを使うものを作る**ときの、フォルダの管理・雛形の使い方・権限の設定をまとめます。文章や企画などの仕事が中心なら、[AI 仕事部屋](05-ai-workroom.md) から始めるのがおすすめです。

> AI への頼み方や、変更の保存（コミット）のしかたは、[AI への頼み方のコツ](07-how-to-ask.md) にまとめています。

## 1. 推奨ディレクトリ構成

「元のリポジトリ（projects）」と「作業用 worktree（workspaces）」を分けて置きます。

```
~/orca/
├── projects/                 # 各プロジェクトの本体（main ブランチ）
│   ├── ai-dev-setup/         # このガイド（雛形の置き場所）
│   ├── my-app/               # 雛形 new-project から作ったプロジェクト
│   ├── my-vault/             # 雛形 obsidian-vault から作った保管庫（任意）
│   └── my-workroom/          # 雛形 ai-workroom から作った AI 仕事部屋（任意）
└── workspaces/               # worktree（タスクごとの作業フォルダ）
    ├── my-app/
    │   ├── login-page/       # feature ブランチ
    │   └── fix-header/
    └── ai-dev-setup/
        └── docs-add-tips/
```

| ルール | 理由 |
|---|---|
| `projects/` の本体では直接作業しない | main を常にきれいに保ち、新しい worktree の起点にする |
| 1タスク = 1 worktree | エージェント同士の変更が混ざらない |
| マージしたら worktree を削除 | フォルダとブランチが溜まり続けるのを防ぐ |
| パスに空白を入れない（特に Windows） | ツールによってはエラーになる |

> Windows の場合は `C:\dev\orca\projects\...` のように短いパスを推奨します。

Orca を使わずに手動で worktree を作る場合（作成 → 一覧 → 削除）：

```zsh
cd ~/orca/projects/my-app
git worktree add ../../workspaces/my-app/login-page -b feature/login-page
git worktree list
git worktree remove ../../workspaces/my-app/login-page
```

```powershell
cd C:\dev\orca\projects\my-app
git worktree add ..\..\workspaces\my-app\login-page -b feature/login-page
git worktree list
git worktree remove ..\..\workspaces\my-app\login-page
```

## 2. リポジトリ内の構成例

このリポジトリ（`ai-dev-setup`）自体がお手本の構成になっています。新しいプロジェクトでは、`templates/new-project/` の中身をコピーして使います。

```
my-app/
├── README.md
├── AGENTS.md          # AI エージェント共通の指示書（コミットする）
├── CLAUDE.md          # Claude Code 用。中身は「@AGENTS.md」の1行だけ
├── .agents/skills/    # スキル（Codex・Antigravity など）→ 11-skills.md
├── .claude/
│   ├── settings.json  # Claude Code の許可・禁止コマンド（チーム共有）
│   └── skills/        # スキル（Claude Code 用。中身は .agents/skills と同じ）
├── .gitignore         # .env・個人用の設定ファイルを除外
├── .gitattributes     # 改行コードを LF に統一
├── .editorconfig      # 文字コード・インデントを統一
├── docs/              # 仕様・設計メモ（AI にも読ませる）
└── src/               # プログラムを書き始めるときに AI が作る（最初はない）
```

| ファイル | ポイント |
|---|---|
| `AGENTS.md` | ルールの本体。Codex などはこのファイルを直接読む |
| `CLAUDE.md` など | エージェントごとの指示ファイルは、`AGENTS.md` を参照するだけにして二重管理を防ぐ |
| `.claude/settings.json` など | エージェントの設定。使うエージェントの分だけ置く |
| `.agents/skills/`・`.claude/skills/` | スキルの置き場所。同じスキルを両方に置く（[11-skills.md](11-skills.md)） |
| `.gitignore` | `.env` と個人用の設定（`CLAUDE.local.md`、`.claude/settings.local.json`）は必ず除外 |
| `.gitattributes` | Windows と Mac が混ざるチームでは必須 |

### テンプレートから新しいプロジェクトを作る

雛形は `ai-dev-setup/templates/` にあります。**中身をまるごと、`ai-dev-setup` の隣にコピー**します。

```
~/orca/projects/
├── ai-dev-setup/templates/new-project/   ← コピー元
└── my-app/                                ← コピー先（新しく作る）
```

#### 方法1: AI に頼む（おすすめ）

Orca で `ai-dev-setup` を開き、エージェントに頼みます。正しい場所に作る手順は、スキル `create-from-template`（[11-skills.md](11-skills.md)）に書いてあります。

```
templates の new-project を使って、my-app という名前で新しいプロジェクトを作って
```

#### 方法2: 自分でコピーする

**1. コピーして、最初のコミットまでを一度に行います。** 次のコマンドは、途中で失敗したらそこで止まります。同じ名前のフォルダがすでにある場合も、上書きせずに止まります。

```zsh
mkdir ~/orca/projects/my-app && cp -R ~/orca/projects/ai-dev-setup/templates/new-project/. ~/orca/projects/my-app/ && git -C ~/orca/projects/my-app init && git -C ~/orca/projects/my-app add . && git -C ~/orca/projects/my-app commit -m "最初のコミット"
```

```powershell
$src = "C:\dev\orca\projects\ai-dev-setup\templates\new-project"; $dest = "C:\dev\orca\projects\my-app"; if (Test-Path $dest) { Write-Host "すでに $dest があります。別の名前にしてください" } else { try { New-Item -ItemType Directory $dest -ErrorAction Stop | Out-Null; Copy-Item -Path "$src\*" -Destination $dest -Recurse -ErrorAction Stop; git -C $dest init; if ($LASTEXITCODE -ne 0) { throw "git init に失敗しました" }; git -C $dest add .; if ($LASTEXITCODE -ne 0) { throw "git add に失敗しました" }; git -C $dest commit -m "最初のコミット"; if ($LASTEXITCODE -ne 0) { throw "git commit に失敗しました" }; Write-Host "完了しました" } catch { Write-Host "失敗しました: $_" } }
```

- 最初のコミットまで行うのは、これをしないと Orca で作る作業フォルダ（worktree）に雛形のファイルが入らないためです
- Mac で `File exists` と出た場合は、同じ名前のフォルダがすでにあります。別の名前にしてやり直してください

> `Please tell me who you are` と出た場合は、名前とメールアドレスの設定がまだです。セットアップの「Git を入れる」にある `git config --global user.name ...` と `git config --global user.email ...` の**両方**を実行してから、`my-app` フォルダで `git commit -m "最初のコミット"` をもう一度実行します。

**2. Orca に登録する。** Orca の「Add project」で `my-app` を登録し、エージェントに次のように頼むと `AGENTS.md` の「〇〇」を埋めてくれます。

```
AGENTS.md の「〇〇」の部分を、作りたいものに合わせて埋めて。わからないところは質問して
```

Obsidian の保管庫を作る場合は、`new-project` を `obsidian-vault`、`my-app` を `my-vault` に読み替えます（[13-obsidian-optional.md](13-obsidian-optional.md)）。AI 仕事部屋の作り方は [05-ai-workroom.md](05-ai-workroom.md) を参照してください。

## 3. 権限設定の考え方

どのエージェントにも、「どのコマンドを確認なしで実行してよいか」を決める設定があります。よく使う安全なコマンドを許可しておくと、確認が減って作業が速くなります。

| 方針 | 例 |
|---|---|
| 許可してよい | `git status`、`git diff` などの確認系、テストの実行など、何も壊さない操作 |
| 毎回確認する | ツールやライブラリの追加、`git push`、外部サービスへの書き込み |
| 禁止する | `rm -rf`、`git push --force`、`.env` の読み取り |

- 取り返しのつかない操作は禁止（deny）にしておく
- 禁止の設定は**最低限の安全策**です。別の書き方のコマンド（例: `rm -fr`）や別のツール経由では防げないことがあるので、「全部許可」に頼りすぎないようにします
- **「全部許可」するモード（確認をすべて省くモード）は使わない**のが基本です。worktree は作業ファイルを分けるだけで、AI ができる操作の範囲は狭まりません（ほかのフォルダや Git の履歴にも手が届きます）。確認を省いてよいのは、コンテナや仮想マシンなど、AI がさわれる範囲を実際に制限した環境だけです
- 設定ファイルの場所と書き方はエージェントごとに違います（例: Claude Code は `.claude/settings.json`、Codex は `~/.codex/config.toml`）。各公式ドキュメントを確認してください

---

<!-- nav-bottom -->
| ← 前へ: [08 AGENTS.md の書き方](08-agents-md.md) | [目次](../README.md#読む順番) | 次へ: [10 便利ツール](10-tools.md) → |
|:---|:---:|---:|
<!-- /nav-bottom -->
