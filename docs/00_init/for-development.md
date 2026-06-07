# 開発情報まとめ
開発にあたり使用するもの、操作・コマンド類の備忘です。

## 前提となる環境
### ローカルにインストールするもの
- VS Code
- Docker Desktop
- Node.js

### Node.js: node, npm
#### インストール
1. node のインストール \
    ローカルにインストールし、パスを通した状態に
    ```bash
    $ node -v
    v24.16.0

    $ npm -v
    11.13.0
    ```

    初期化し、ルートに`package.json`を生成
    ```bash
    $ npm init -y
    ```

## DB
### dbdiagram
- テーブル設計、ERD 作成に使用
- <https://dbdiagram.io/>への登録が必要
- 設計書として`.dbml`ファイルを管理
- VS Code extension
  - [dbdiagram](https://marketplace.visualstudio.com/items?itemName=dbdiagram.dbdiagram-vscode)

## API
### OpenAPI
- API デザインの標準仕様として使用
- VS Code extension
  - [OpenAPI (Swagger) Editor](https://marketplace.visualstudio.com/items?itemName=42Crunch.vscode-openapi)
  - [YAML](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml)

### redocly
- OpenAPI Spec.yaml の linter やドキュメントマージ、html 化に使える node アプリ

#### インストール
1. redocly のインストール
    ```bash
    $ npm install -D @redocly/cli
    ```

    `package.json`に以下を追加 \
    コマンド名やパスは適宜修正する
    ```json:package.json
    {
      "scripts": {
        "api:lint": "redocly lint docs/bff/references/divstack.v1.yaml",
        "api:bundle": "redocly bundle docs/bff/references/divstack.v1.yaml -o dist/bff/divstack.v1.yaml",
        "api:docs": "redocly build-docs docs/bff/references/divstack.v1.yaml -o dist/bff/api-docs.html"
      }
    }
    ```

1. `.gitignore`への追加
    ```.gitignore
    node_modules
    dist/*
    ```

1. redocly の実行
    ```bash
    # OpenAPI.yaml のlinterをかける
    $ npm run api:lint

    # 複数分割されたyamlを1つのOpenAPI.yamlにまとめる
    $ npm run api:bundle

    # API仕様書.html として出力する
    $ npm run api:docs
    ```

## 文書作成
### textlint
- 文書の linter
- VS Code extension
  - [textlint](https://marketplace.visualstudio.com/items?itemName=3w36zj6.textlint)
  - `taichi.vscode-textlint`は更新停止のため使用しない

#### インストール
1. textlint のインストール
    ```bash
    $ npm install -D textlint
    ...

    $ npx textlint --init
    (.textlintrc が生成)
    ```
1. textlint の plugins インストール
    ```bash
    # コメントでルール無効化範囲を指定
    $ npm install -D textlint-filter-rule-comments

    # 技術文書向けプリセットルール
    $ npm install -D textlint-rule-preset-ja-technical-writing

    # 箇条書きの末尾に句点やピリオドを許容しない
    $ npm install -D textlint-rule-period-in-list-item

    # 日本語におけるスペースの扱い
    $ npm install -D textlint-rule-preset-ja-spacing

    # 全角と半角アルファベットの混在チェック
    $ npm install -D textlint-rule-no-mixed-zenkaku-and-hankaku-alphabet

    # 表記揺れの管理
    $ npm install -D textlint-rule-prh
    ```

1. 設定ファイル`.textlintrc.jsonc`に追記 \
   `.jsonc`ファイルに変更しているが、そのままでもよい

1. `package.json`に以下を追加
    ```json:package.json
    {
      "scripts": {
        "textlint": "textlint --config .textlintrc.jsonc **/*.md",
      }
    }
    ```

### markdownlint
- markdown ファイルの linter
- VS Code extension
  - [markdownlint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint)

#### インストール
1. markdownlint のインストール
    ```bash:
    $ npm install -D markdownlint-cli2
    ```

1. 設定ファイル`.markdownlint.jsonc`に変更したいルール設定を追記

1. 設定ファイル`.markdownlint-cli2.jsonc`に実行対象ファイルを記載

1. `package.json`に以下を追加
    ```json:package.json
    {
      "scripts": {
        "markdownlint": "markdownlint-cli2",
      }
    }
    ```

### Code Spell Checker (cSpell)
- typo 検出用のスペルチェッカー
- VS Code extension
  - [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)

#### インストール
1. cSpell のインストール
    ```bash
    $ npm install -D cspell@latest
    ```

1. 設定ファイル`cspell.jsonc`に設定を追記

1. `package.json`に以下を追加
    ```json:package.json
    {
      "scripts": {
        "cspell": "cspell lint **/*.md",
      }
    }
    ```

## プロセス・ルール遵守
### 事前準備
- `package.json`に記載の script を整理
  - 必要なコマンドを束ねる

### commitlint
- `git commit`のコメント記述ルールの強制
- 後述の husky と合わせて使用する

#### インストール
1. commitlint のインストール
    ```bash
    $ npm install -D @commitlint/cli @commitlint/config-conventional
    ```

1. 設定ファイル`.commitlintrc.yaml`にコミットメッセージのルールを記

### Husky
- `git commit`や`git push`などの git 操作を hook に任意のスクリプトを実行するツール
- linter や test の実行に使用

#### インストール
1. husky のインストール
    ```bash
    $ npm install -D husky
    ...

    $ npx husky init
    (.husky/ ディレクトリが生成される)
    ```

2. `.husky/`配下の hook ファイルを整備
    ```bash
    ${ROOT}/.husky
    │  commit-msg   # commitlint 実行
    │  pre-commit   # commit 時の hook コマンド
    │  pre-push     # push 時の hook コマンド
    │
    └─_  (省略)
    ```

## GitHub 関連
TODO あとで書く。
- GitHub Actions
- Pull Request Template
- Issue Template
- commitlint（既出）
