# templates（雛形）

新しく何かを始めるときに、**中身をまるごとコピーして使う**フォルダです。このフォルダの中では作業しません。

| 雛形 | 使う場面 | コピー先の例 |
|---|---|---|
| [`new-project/`](new-project/) | アプリ・ツール・Web サイトなどを作るとき | `~/orca/projects/my-app/` |
| [`obsidian-vault/`](obsidian-vault/) | Obsidian でメモ・ナレッジを管理するとき | `~/orca/projects/my-vault/` |
| [`consulting-workspace/`](consulting-workspace/) | SEO・ブランディング・企画・デザインなど、相談から成果物を作るとき（[説明](../docs/10-consulting-workspace.md)） | `~/orca/projects/my-workspace/` |

> Windows の場合、コピー先は `C:\dev\orca\projects\my-app\` のようになります。

## コピーのイメージ

```
~/orca/projects/
├── ai-dev-setup/                 ← このガイド（コピー元）
│   └── templates/
│       ├── new-project/          ──┐
│       ├── obsidian-vault/       ──┼─┐
│       └── consulting-workspace/ ──┼─┼─┐
├── my-app/            ←────────────┘ │ │  新しいプロジェクト
├── my-vault/          ←──────────────┘ │  Obsidian の保管庫
└── my-workspace/      ←────────────────┘  相談ワークスペース
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
templates の consulting-workspace を使って、my-workspace という名前で相談ワークスペースを作って
```

自分でコピーする手順は [05-directory-and-tips.md](../docs/05-directory-and-tips.md#テンプレートから新しいプロジェクトを作る) を参照してください。
