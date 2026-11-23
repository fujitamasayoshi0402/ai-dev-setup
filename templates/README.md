# templates（雛形）

新しく何かを始めるときに、**中身をまるごとコピーして使う**フォルダです。このフォルダの中では作業しません。

| 雛形 | 使う場面 | コピー先の例 |
|---|---|---|
| [`ai-workroom/`](ai-workroom/) | **AI 仕事部屋**。企画・文章・SNS・調べもの・SEO など、AI に仕事を手伝ってもらうとき（[説明](../docs/05-ai-workroom.md)） | `~/orca/projects/my-workroom/` |
| [`new-project/`](new-project/) | アプリ・ツール・Web サイトなどを作るとき | `~/orca/projects/my-app/` |
| [`obsidian-vault/`](obsidian-vault/) | Obsidian でメモ・ナレッジを管理するとき | `~/orca/projects/my-vault/` |

> Windows の場合、コピー先は `C:\dev\orca\projects\my-app\` のようになります。

## コピーのイメージ

```
~/orca/projects/
├── ai-dev-setup/                 ← このガイド（コピー元）
│   └── templates/
│       ├── new-project/          ──┐
│       ├── obsidian-vault/       ──┼─┐
│       └── ai-workroom/          ──┼─┼─┐
├── my-app/            ←────────────┘ │ │  新しいプロジェクト
├── my-vault/          ←──────────────┘ │  Obsidian の保管庫
└── my-workroom/       ←────────────────┘  AI 仕事部屋
```

**ポイント:** コピー先は `ai-dev-setup` の中ではなく、**隣**（同じ `projects/` の下）です。

## いちばん簡単な方法: AI に頼む

Orca で `ai-dev-setup` を開き、エージェントに次のように頼みます。手順はスキル `create-from-template` に書いてあるので、正しい場所に作ってくれます。

```
templates の new-project を使って、my-app という名前で新しいプロジェクトを作って
```

```
templates の obsidian-vault を使って、my-vault という名前で Obsidian の保管庫を作って
```

```
templates の ai-workroom を使って、my-workroom という名前で AI 仕事部屋を作って
```

自分でコピーする手順は [09-build-projects.md](../docs/09-build-projects.md#テンプレートから新しいプロジェクトを作る) を参照してください。
