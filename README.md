# {Your Marketplace Name}

Claude Code プラグインマーケットプレイステンプレート。

このリポジトリをテンプレートとして使用し、組織やチーム向けのプラグインマーケットプレイスを構築できます。

## セットアップ

1. このリポジトリをテンプレートとして使用し、新しいリポジトリを作成
2. 以下のファイルを編集して、自分のマーケットプレイスに合わせてカスタマイズ:
   - `.claude-plugin/marketplace.json` - マーケットプレイス名、オーナー情報、プラグイン一覧
   - `README.md` - このファイルを自分のマーケットプレイスの説明に書き換える

## ディレクトリ構成

```
.
├── .claude-plugin/
│   └── marketplace.json      # マーケットプレイスメタデータ
├── plugins/
│   └── <plugin-name>/        # 各プラグイン
│       ├── .claude-plugin/
│       │   └── plugin.json   # プラグインメタデータ
│       ├── commands/         # スラッシュコマンド
│       ├── agents/           # カスタムエージェント
│       └── skills/           # エージェントスキル
└── README.md
```

## インストール

### マーケットプレイスを追加

```bash
# GitHubリポジトリから追加
/plugin marketplace add <owner>/<repo>

# ローカル開発時
/plugin marketplace add ./path/to/marketplace
```

### プラグインをインストール

```bash
/plugin install <plugin-name>@<marketplace-name>
```

## プラグインの追加方法

### 1. プラグインディレクトリを作成

```bash
mkdir -p plugins/<plugin-name>/.claude-plugin
mkdir -p plugins/<plugin-name>/commands
mkdir -p plugins/<plugin-name>/agents
mkdir -p plugins/<plugin-name>/skills
```

### 2. plugin.json を作成

`plugins/<plugin-name>/.claude-plugin/plugin.json`:

```json
{
  "name": "plugin-name",
  "version": "1.0.0",
  "description": "プラグインの説明",
  "author": {
    "name": "作者名"
  },
  "keywords": ["keyword1", "keyword2"],
  "commands": "./commands/",
  "agents": "./agents/",
  "skills": "./skills/"
}
```

### 3. コマンド・エージェント・スキルを追加

- **commands/**: Markdown ファイルでスラッシュコマンドを定義
- **agents/**: Markdown ファイルでカスタムエージェントを定義
- **skills/**: `<skill-name>/SKILL.md` でスキルを定義

### 4. marketplace.json を更新

`.claude-plugin/marketplace.json` の `plugins` 配列にエントリを追加:

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

### 5. 検証

```bash
claude plugin validate .
```

## 外部プラグインの取り込み

マーケットプレイスは自前のプラグインだけでなく、外部リポジトリの優れたプラグインをキュレーションして配布する場としても活用できます。

### GitHubリポジトリ

```json
{
  "name": "external-plugin",
  "source": {
    "source": "github",
    "repo": "owner/plugin-repo"
  },
  "description": "外部プラグインの説明",
  "version": "1.0.0"
}
```

### 任意の Git URL

```json
{
  "name": "external-plugin",
  "source": {
    "source": "url",
    "url": "https://github.com/org/plugin-repo.git"
  },
  "description": "外部プラグインの説明",
  "version": "1.0.0"
}
```

## 同梱プラグイン

### example-plugin

コマンド、エージェント、スキルの実装例を含むデモプラグインです。

| 種別 | 名前 | 説明 |
|------|------|------|
| コマンド | `/hello` | ユーザーに挨拶 |
| コマンド | `/summarize` | コンテンツを要約 |
| エージェント | `security-reviewer` | セキュリティ観点のコードレビュー |
| エージェント | `test-generator` | TDD テストケース生成 |
| スキル | `code-review` | コードレビューのベストプラクティス |

## License

MIT
