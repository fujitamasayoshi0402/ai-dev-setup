# Obsidian（オプション）

> このページは任意です。AI 駆動開発自体は Obsidian なしでも問題なく行えます。

[Obsidian](https://obsidian.md/) は Markdown ファイルでメモを管理するアプリです。データがただの `.md` ファイルなので、**AI エージェントがそのまま読み書きできる**のが最大の利点です。

## 何に使うと便利か

| 用途 | 例 |
|---|---|
| 調査メモの蓄積 | AI に調べさせた結果を `.md` で保存し、後から検索 |
| 日報・作業ログ | その日 AI に頼んだこと・学んだことを記録 |
| プロンプト集 | よく使う指示文をテンプレート化 |
| 設計メモ | リポジトリの `docs/` を Vault として開き、図やリンクで整理 |

## インストール

### Mac

```zsh
brew install --cask obsidian
```

### Windows

```powershell
winget install --id Obsidian.Obsidian -e
```

## Vault（保管庫）の作り方

おすすめは2パターンです。どちらも、Orca に登録すれば AI エージェントと一緒に使えます。

### A. 個人ナレッジ用の Vault（雛形あり）

雛形 `templates/obsidian-vault/` を使います。置き場所は、ほかのプロジェクトと同じ `orca/projects/` の下です。

```
~/orca/projects/
├── ai-dev-setup/templates/obsidian-vault/   ← コピー元
└── my-vault/                                 ← コピー先（新しく作る）
    ├── 00_inbox/        # とりあえず放り込む（AI が作るメモもここ）
    ├── 10_daily/        # 日報（YYYY-MM-DD.md）
    ├── 20_research/     # 調べたことのまとめ
    ├── 30_prompts/      # よく使う AI への指示文
    ├── 40_projects/     # プロジェクトごとのメモ
    ├── 90_templates/    # 日報・調査メモ・プロンプトのテンプレート
    ├── AGENTS.md        # この保管庫での AI のルール
    ├── CLAUDE.md        # @AGENTS.md の1行だけ
    └── README.md        # 最初にやる Obsidian の設定
```

#### 作り方

1. Orca で `ai-dev-setup` を開き、エージェントに頼みます（自分でコピーする場合は [05-directory-and-tips.md](05-directory-and-tips.md#テンプレートから新しいプロジェクトを作る) の手順を参照）
   ```
   templates の obsidian-vault を使って、my-vault という名前で Obsidian の保管庫を作って
   ```
2. Obsidian で「フォルダを保管庫として開く」から `my-vault` を開きます
3. `my-vault/README.md` の「最初にやること」に沿って、テンプレートとデイリーノートを設定します
4. Orca の「Add project」で `my-vault` を登録します

これで、Orca から保管庫でエージェントを起動し、次のように頼めます。

```
今週の日報（10_daily）を読んで、やったことを5行でまとめて
```

```
〇〇について調べて、20_research に調査メモのテンプレートの形でまとめて
```

```
00_inbox のメモを読んで、どのフォルダに移すとよいか提案して
```

> 保管庫も Git で管理されるので、AI がメモを壊しても元に戻せます。GitHub を使うなら、**非公開（Private）リポジトリ**にバックアップしましょう。

### B. プロジェクトの docs をそのまま開く

Obsidian で「フォルダを保管庫として開く」から `my-app/docs/` を選ぶだけです。仕様書を Obsidian で読み書きし、同じファイルを AI も参照できます。

> `.obsidian/` フォルダ（個人設定）は `.gitignore` に入れておきましょう（雛形 `new-project` には設定済みです）。

## おすすめ設定・プラグイン

| 項目 | 内容 |
|---|---|
| 設定 → ファイルとリンク → 「Wikiリンクを使用」 | パターン B（GitHub でも読む docs）ではオフにする。標準の Markdown リンクになり、GitHub 上でも表示が崩れない |
| コアプラグイン「デイリーノート」 | 日報を1クリックで作成（雛形では `10_daily` に保存） |
| コアプラグイン「テンプレート」 | 調査メモ・日報の雛形（雛形では `90_templates` を使う） |
| コミュニティプラグイン「Obsidian Git」 | Vault を GitHub に自動バックアップ |

## 運用のコツ

- **AI の出力は inbox に入れて、人が整理する**。全部 AI 任せにするとノイズが増える
- 調べた結果は「結論 → 根拠 → 参考URL」の順で書かせると後で読みやすい
- 機密情報を含む Vault は公開リポジトリに push しない
- 最初は `00_inbox` と `10_daily` だけ使えば十分。慣れたら他のフォルダも使う
