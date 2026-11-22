# ディレクトリ管理と Tips

## 1. 推奨ディレクトリ構成

「元のリポジトリ（projects）」と「作業用 worktree（workspaces）」を分けて置きます。

```
~/orca/
├── projects/                 # 各プロジェクトの本体（main ブランチ）
│   ├── ai-dev-setup/         # このガイド（雛形の置き場所）
│   ├── my-app/               # 雛形 new-project から作ったプロジェクト
│   └── my-vault/             # 雛形 obsidian-vault から作った保管庫（任意）
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
├── .agents/skills/    # スキル（Codex・Antigravity など）→ 07-skills.md
├── .claude/
│   ├── settings.json  # Claude Code の許可・禁止コマンド（チーム共有）
│   └── skills/        # スキル（Claude Code 用。中身は .agents/skills と同じ）
├── .gitignore         # .env・個人用の設定ファイルを除外
├── .gitattributes     # 改行コードを LF に統一
├── .editorconfig      # 文字コード・インデントを統一
├── docs/              # 仕様・設計メモ（AI にも読ませる）
└── src/
```

| ファイル | ポイント |
|---|---|
| `AGENTS.md` | ルールの本体。Codex などはこのファイルを直接読む |
| `CLAUDE.md` など | エージェントごとの指示ファイルは、`AGENTS.md` を参照するだけにして二重管理を防ぐ |
| `.claude/settings.json` など | エージェントの設定。使うエージェントの分だけ置く |
| `.agents/skills/`・`.claude/skills/` | スキルの置き場所。同じスキルを両方に置く（[07-skills.md](07-skills.md)） |
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

Orca で `ai-dev-setup` を開き、エージェントに頼みます。正しい場所に作る手順は、スキル `create-from-template`（[07-skills.md](07-skills.md)）に書いてあります。

```
templates の new-project を使って、my-app という名前で新しいプロジェクトを作って
```

#### 方法2: 自分でコピーする

**1. コピーする。** 同じ名前のフォルダがすでにある場合は、上書きしないように何もせず止まります。

```zsh
mkdir ~/orca/projects/my-app && cp -R ~/orca/projects/ai-dev-setup/templates/new-project/. ~/orca/projects/my-app/
```

```powershell
$dest = "C:\dev\orca\projects\my-app"; if (Test-Path $dest) { Write-Host "すでに $dest があります。別の名前にしてください" } else { New-Item -ItemType Directory $dest | Out-Null; Copy-Item -Path C:\dev\orca\projects\ai-dev-setup\templates\new-project\* -Destination $dest -Recurse; Write-Host "コピーしました" }
```

> Mac で `File exists` と出た場合も、同じ名前のフォルダがすでにあります。別の名前にしてやり直してください。

**2. Git で管理を始めて、最初のコミットをする。** これをしないと、Orca で作る作業フォルダ（worktree）に雛形のファイルが入りません。

```zsh
cd ~/orca/projects/my-app
git init
git add .
git commit -m "最初のコミット"
```

```powershell
cd C:\dev\orca\projects\my-app
git init
git add .
git commit -m "最初のコミット"
```

> `Please tell me who you are` と出た場合は、名前とメールアドレスの設定がまだです。セットアップの「Git を入れる」にある `git config --global user.name ...` を実行してから、もう一度 `git commit` します。

**3. Orca に登録する。** Orca の「Add project」で `my-app` を登録し、エージェントに次のように頼むと `AGENTS.md` の「〇〇」を埋めてくれます。

```
AGENTS.md の「〇〇」の部分を、作りたいものに合わせて埋めて。わからないところは質問して
```

Obsidian の保管庫を作る場合は、`new-project` を `obsidian-vault`、`my-app` を `my-vault` に読み替えます（[06-obsidian-optional.md](06-obsidian-optional.md)）。

## 3. 変更の保存（コミット）と作業の分け方

Git の操作は AI に頼めば大丈夫です。覚えておくのは「いつ頼むか」だけです。

| タイミング | AI への頼み方の例 |
|---|---|
| ひと区切りついたとき | 「ここまでの変更をコミットして」 |
| おかしくなったとき | 「さっきのコミットの状態に戻して」 |
| 新しい作業を始めるとき | Orca で新しい worktree を作る（または「新しいブランチを作って」） |
| 作業が終わったとき | 「この変更を main に取り込んで」 |

- コミットは小さく、こまめに。戻れる地点が増えるほど安心して AI に任せられます
- ブランチ名の例: `feature/xxx`（機能追加）、`fix/xxx`（修正）、`docs/xxx`（文書）
- （GitHub を使う場合）main への直接 push は禁止し、必ず PR を経由する（GitHub のブランチ保護を設定）

## 4. AI への指示のコツ

### 良い指示の型

```
【目的】何をしたいか（例: 家計簿を記録できる Web ページを作りたい）
【条件】守ってほしいこと（例: スマホでも見やすく / データはこのパソコンの中だけに保存）
【完了条件】何ができたら終わりか（例: 入力した内容が一覧に表示される）
【やらないこと】今回は手を出さないこと（例: ログイン機能は作らない）
```

> 何を書けばいいかわからないときは、「〇〇を作りたい。何を決めればいいか質問して」と頼むと、AI が必要なことを聞いてくれます。

### Tips 集

| Tips | 内容 |
|---|---|
| まず計画させる | 大きな作業は「まず計画だけ出して。実装はまだしないで」と頼み、確認してから進める（計画モードがあるエージェントも多い） |
| 完了条件を明示する | 「〇〇が動くまで」「エラーがなくなるまで」と書くと、AI が自分で確認しながら進めてくれる |
| タスクが変わったら新しい会話に | 古い話が残っていると精度が落ちる。会話のリセットや新しいタブを使う |
| 繰り返し伝えることは指示ファイルへ | 毎回言っていることは `AGENTS.md` などにルールとして書く |
| 手順化できる作業はまとめておく | よくやる手順は Skill やルールとして保存して再利用する |
| 結果は必ず自分で確認する | 画面で動かしてみる・変更内容を見る。わからなければ「何を変えたか説明して」と聞く |
| 別のエージェントに見てもらう | エージェントA が作ったものを、エージェントB にチェックしてもらう（[08-multi-agent.md](08-multi-agent.md)） |
| 秘密情報を貼らない | パスワードや API キーはチャットに直接書かない。`.env` ファイルに置く |
| わからないことは AI に聞く | エラー文・専門用語・手順など、なんでも「わかりやすく説明して」と聞いてよい |

## 5. 権限設定の考え方

どのエージェントにも、「どのコマンドを確認なしで実行してよいか」を決める設定があります。よく使う安全なコマンドを許可しておくと、確認が減って作業が速くなります。

| 方針 | 例 |
|---|---|
| 許可してよい | `git status`、`git diff` などの確認系、テストの実行など、何も壊さない操作 |
| 毎回確認する | ツールやライブラリの追加、`git push`、外部サービスへの書き込み |
| 禁止する | `rm -rf`、`git push --force`、`.env` の読み取り |

- 取り返しのつかない操作は禁止（deny）にしておく
- 禁止の設定は**最低限の安全策**です。別の書き方のコマンド（例: `rm -fr`）や別のツール経由では防げないことがあるので、「全部許可」に頼りすぎないようにします
- 「全部許可」するモードは、worktree の中など影響範囲が限られた場所でだけ使う
- 設定ファイルの場所と書き方はエージェントごとに違います（例: Claude Code は `.claude/settings.json`、Codex は `~/.codex/config.toml`）。各公式ドキュメントを確認してください
