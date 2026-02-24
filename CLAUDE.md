## プロジェクト概要

このリポジトリは、Claude Codeプラグインを一元管理・配布するためのマーケットプレイスです。
組織やチームで共有するプラグインの管理基盤として利用できます。

## ディレクトリ構成

```
marketplace/
├── .claude-plugin/
│   └── marketplace.json    # マーケットプレイスメタデータ
├── .claude/
│   └── rules/              # 開発ルール
├── plugins/                # プラグイン格納ディレクトリ
│   └── <plugin-name>/
│       ├── .claude-plugin/
│       │   └── plugin.json
│       ├── commands/
│       ├── agents/
│       └── skills/
└── README.md
```

## marketplace.json の構造

```json
{
  "name": "your-marketplace",
  "owner": {
    "name": "your-org",
    "url": "https://github.com/your-org"
  },
  "metadata": {
    "description": "マーケットプレイスの説明",
    "version": "1.0.0"
  },
  "plugins": []
}
```

## 開発ルール

### 開発プロセス

プラグインやスキルを実装する際は、以下の優先順位でドキュメントを参照すること：

1. claude-code-guide エージェント: 最新の公式ドキュメントを参照
2. rules ディレクトリ: プロジェクト固有のルールを参照

### プラグイン命名規則

- kebab-case 形式を使用（例: `code-formatter`, `deployment-tools`）
- スペースは使用不可
- ユニークな識別子を使用

### バージョニング

- セマンティックバージョニング（MAJOR.MINOR.PATCH）に従う
- MAJOR: 破壊的変更
- MINOR: 後方互換性のある新機能
- PATCH: バグ修正
- プラグインを変更したPRでは、必ず該当プラグインの `version` を上げること。Claude Codeは各プラグインの `version`（`plugin.json` または `marketplace.json` の plugins エントリ内）をキーにキャッシュするため、バージョンを上げないとユーザーに変更が反映されない

### Author（著者）

- plugin.json の author は、そのプラグインにコミットした人の名前を設定する
- `git log --format="%aN" -- plugins/<plugin-name>/ | sort -u` でコミット履歴から著者を取得
- 複数の著者がいる場合は、カンマ区切りで記載する（例: `"name": "author1, author2"`）

### marketplace.json の更新

プラグインを追加・更新・削除した場合は、必ず `marketplace.json` も更新すること。

#### 更新手順

1. `plugins/<plugin-name>/.claude-plugin/plugin.json` の内容を確認
2. `.claude-plugin/marketplace.json` の `plugins` 配列を更新
   - 新規追加の場合: 配列に新しいエントリを追加（アルファベット順でソート）
   - 更新の場合: 該当エントリの `description`, `version`, `author`, `keywords` を更新
   - 削除の場合: 該当エントリを配列から削除
3. `metadata.version` をインクリメント（patch バージョンを +1）。キャッシュには使われないが、マーケットプレイス全体の変更履歴を追跡するために運用上必須とする
4. `claude plugin validate .` で検証

### plugins 配列のエントリ形式

プラグインのソースは、ローカルパスと外部gitリポジトリの2種類が指定できる。

ローカルパス（このリポジトリ内のプラグイン）:

```json
{
  "name": "plugin-name",
  "source": "./plugins/plugin-name",
  "description": "プラグインの説明",
  "version": "1.0.0",
  "author": {
    "name": "作者名"
  },
  "keywords": ["keyword1", "keyword2"]
}
```

外部gitリポジトリ（URL指定）:

```json
{
  "name": "external-plugin",
  "source": {
    "source": "url",
    "url": "https://github.com/org/plugin-repo.git"
  },
  "description": "外部リポジトリのプラグイン",
  "version": "1.0.0"
}
```

GitHubリポジトリ:

```json
{
  "name": "github-plugin",
  "source": {
    "source": "github",
    "repo": "owner/plugin-repo"
  },
  "description": "GitHubリポジトリのプラグイン",
  "version": "1.0.0"
}
```

### プラグインのソースについて

マーケットプレイスは、自前でプラグインを実装するだけの場にとどまらない。外部の優れたスキルやプラグインを見つけて取り込むキュレーションの場でもある。

- 自前実装: `plugins/` ディレクトリ内にプラグインを作成し、`source` にローカルパスを指定
- 外部から取り込む: 良質なスキルやプラグインを持つ外部gitリポジトリを `source` に指定して配布
  - GitHub.com 上のOSSプラグイン
  - 任意のgitホスト（URL末尾が `.git` であれば対応）
- ブランチ・タグ固定: `ref` や `sha` で特定バージョンにピン留め可能

### plugin.json の形式

各プラグインの `.claude-plugin/plugin.json` は以下の形式で記述する：

```json
{
  "name": "plugin-name",
  "version": "1.0.0",
  "description": "プラグインの説明",
  "author": {
    "name": "作者名"
  },
  "keywords": ["keyword1", "keyword2"]
}
```

## よく使うコマンド

```bash
# マーケットプレイスを検証
claude plugin validate .

# プラグインをローカルでテスト
claude --plugin-dir ./plugins/<plugin-name>

# マーケットプレイスをローカルで追加
/plugin marketplace add .
```
