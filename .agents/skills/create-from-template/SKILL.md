---
name: create-from-template
description: ai-dev-setup の templates/ にある雛形（new-project / obsidian-vault）から、新しいプロジェクトや Obsidian の保管庫を正しい場所に作る。「雛形から〇〇を作って」「新しいプロジェクトを作って」「Obsidian の保管庫を作って」と頼まれたときに使う。
---

# 雛形から新しいプロジェクト・保管庫を作る

利用者はプログラミング経験がない場合があるので、コピーや Git の操作は代わりに行う。

## 手順

1. **雛形を決める**（はっきりしなければ聞く）
   - アプリ・ツール・Web サイトなど → `templates/new-project/`
   - Obsidian でメモを管理する → `templates/obsidian-vault/`
2. **名前を決める**。決まっていなければ聞く。英小文字・数字・ハイフンのみ（例: `my-app`、`my-vault`）
3. **コピー先を決める**。ai-dev-setup の**元の場所（本体）の隣**にする（ai-dev-setup の中には作らない）
   - 今いるフォルダが Orca の worktree（`orca/workspaces/` の下）の場合があるので、まず `git worktree list --porcelain` を実行し、**最初に表示される `worktree` の行**を本体の場所とする
   - 本体が `~/orca/projects/ai-dev-setup` なら → `~/orca/projects/<名前>/`
   - Windows で `C:\dev\orca\projects\ai-dev-setup` なら → `C:\dev\orca\projects\<名前>\`
   - `orca/workspaces/` の下には**作らない**
4. **コピー先がすでにあれば止まる**。上書きせず、別の名前にするか利用者に確認する
5. **コピーする**。`.gitignore` など `.` で始まるファイルとフォルダも含めて、雛形の中身をまるごとコピーする
   - Mac: `mkdir <コピー先> && cp -R <雛形>/. <コピー先>/`
   - Windows: `mkdir <コピー先>; Copy-Item -Path <雛形>\* -Destination <コピー先> -Recurse -Force`
6. **確認する**。コピー先に `AGENTS.md`・`CLAUDE.md`・`.gitignore` があることを確かめる
7. **Git の準備**。コピー先で `git init` → `git add .` → `最初のコミット` というメッセージでコミットする
   - `git config user.name` と `git config user.email` が未設定でコミットできない場合は、利用者に名前とメールアドレスを聞いて `git config --global` で設定してから再実行する（GitHub を使う予定なら、GitHub の非公開用アドレス `xxxx+ユーザー名@users.noreply.github.com` をすすめる）
8. **中身を埋める**（`new-project` の場合）。作りたいものを利用者に聞き、`AGENTS.md` と `README.md` の「〇〇」を埋めてコミットする。決まっていない項目は「〇〇」のまま残し、そのことを伝える
9. **次にやることを伝える**
   - 作った場所（フルパス）
   - Orca の「Add project」でそのフォルダを登録すること
   - `obsidian-vault` の場合は、Obsidian で「フォルダを保管庫として開く」から開き、保管庫の `README.md` の初期設定を行うこと
