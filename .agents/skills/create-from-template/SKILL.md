---
name: create-from-template
description: ai-dev-setup の templates/ にある雛形（ai-workroom / new-project / obsidian-vault）から、AI 仕事部屋や新しいプロジェクト、Obsidian の保管庫を正しい場所に作る。「雛形から〇〇を作って」「新しいプロジェクトを作って」「Obsidian の保管庫を作って」「AI 仕事部屋を作って」と頼まれたときに使う。
---

# 雛形から新しいプロジェクト・保管庫を作る

利用者はプログラミング経験がない場合があるので、コピーや Git の操作は代わりに行う。

## 手順

1. **雛形を決める**（はっきりしなければ聞く）
   - アプリ・ツール・Web サイトなど → `templates/new-project/`
   - Obsidian でメモを管理する → `templates/obsidian-vault/`
   - AI に仕事（企画・文章・ブログ・SNS・調べもの・SEO・デザインの準備など）を手伝ってもらう → `templates/ai-workroom/`（名前の例: `my-workroom`）
2. **名前を決める**。決まっていなければ聞く。英小文字・数字・ハイフンのみ（例: `my-app`、`my-vault`）
3. **コピー先を決める**。ai-dev-setup の**元の場所（本体）の隣**にする（ai-dev-setup の中には作らない）
   - 今いるフォルダが Orca の worktree（`orca/workspaces/` の下）の場合があるので、まず `git worktree list --porcelain` を実行し、**最初に表示される `worktree` の行**を本体の場所とする
   - Git で管理されていない（ZIP でダウンロードした）場合はこのコマンドが失敗する。そのときは、`AGENTS.md` と `templates/` がある ai-dev-setup のフォルダを本体とする
   - 本体が `~/orca/projects/ai-dev-setup` なら → `~/orca/projects/<名前>/`
   - Windows で `C:\dev\orca\projects\ai-dev-setup` なら → `C:\dev\orca\projects\<名前>\`
   - `orca/workspaces/` の下には**作らない**
4. **コピー先がすでにあれば止まる**。上書きせず、別の名前にするか利用者に確認する
5. **コピーする**。`.gitignore` など `.` で始まるファイルとフォルダも含めて、雛形の中身をまるごとコピーする。**失敗したら次に進まない**
   - Mac: `mkdir <コピー先> && cp -R <雛形>/. <コピー先>/`
   - Windows: `New-Item -ItemType Directory <コピー先> -ErrorAction Stop` が成功したときだけ `Copy-Item -Path <雛形>\* -Destination <コピー先> -Recurse -ErrorAction Stop`（`-Force` は付けない）
   - 以降の Git 操作は、移動に失敗して別の場所で実行しないよう `git -C <コピー先> ...` の形で行う
6. **確認する**。コピー先に `AGENTS.md`・`CLAUDE.md`・`.gitignore` があることを確かめる
7. **Git の準備**。コピー先で `git init` → `git add .` → `最初のコミット` というメッセージでコミットする
   - `git config user.name` と `git config user.email` が未設定でコミットできない場合は、利用者に名前とメールアドレスを聞いて `git config --global` で設定してから再実行する（GitHub を使う予定なら、GitHub の非公開用アドレス `xxxx+ユーザー名@users.noreply.github.com` をすすめる）
8. **中身を埋める**（`new-project` の場合）。`AGENTS.md` の「〇〇」（何を作るか・誰のためか・大事にしたいこと・完了の条件）を、**専門用語を使わずに1つずつ質問して**埋め、`README.md` の「〇〇」も埋めてコミットする
   - 決まっていない項目は「〇〇」のまま残し、そのことを伝える
   - 「技術メモ」は利用者に聞かない。使う技術が決まった時点で AI が書く
9. **次にやることを伝える**
   - 作った場所（フルパス）
   - Orca の「Add project」でそのフォルダを登録すること
   - `obsidian-vault` の場合は、Obsidian で「フォルダを保管庫として開く」から開き、保管庫の `README.md` の初期設定を行うこと
   - `ai-workroom` の場合は、作業フォルダ（worktree）を作らずに部屋そのものでエージェントを起動すること、そして最初に `about/profile.md` と `brand.md` を埋めること（部屋で「about の profile.md と brand.md を、質問しながら一緒に埋めて」と頼めばよい）
