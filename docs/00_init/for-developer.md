# 開発情報まとめ

開発にあたり使用するもの、操作・コマンド類の備忘です。

## DB

### dbdiagram

- テーブル設計、ERD作成に使用
- <https://dbdiagram.io/> への登録が必要
- 設計書として.dbmlファイルを管理
- vscode extention
  - [dbdiagram](https://marketplace.visualstudio.com/items?itemName=dbdiagram.dbdiagram-vscode)

## API

### OpenAPI

- APIデザインの標準仕様として使用
- vscode extention
  - [OpenAPI (Swagger) Editor](https://marketplace.visualstudio.com/items?itemName=42Crunch.vscode-openapi)
  - [YAML](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml)

### redocly

- OpenAPIドキュメント.yaml の linter や ドキュメントマージ、html化に使える node アプリ

#### インストール

1. node のインストール

    ```bash
    $ node -v
    v24.16.0

    $ npm -v
    11.13.0
    ```

1. redocly のインストール

    ```bash
    $ npm init -y
    ...
    ```

    ルートに `package.json` が生成される

    ```json:package.json
    {
      "name": "divstack",
      "version": "1.0.0"
    }
    ```

    redocly インストール

    ```bash
    $ npm install -D @redocly/cli
    ...
    ```

    `package.json` に以下を追加 \
    コマンドやOpenAPI.yamlへのパスは適宜修正する

    ```json:package.json
    {
      "scripts": {
        "bff:lint": "redocly lint docs/bff/references/divstack.v1.yaml",
        "bff:bundle": "redocly bundle docs/bff/references/divstack.v1.yaml -o dist/bff/divstack.v1.yaml",
        "bff:docs": "redocly build-docs docs/bff/references/divstack.v1.yaml -o dist/bff/api-docs.html"
      }
    }
    ```

    `.gitignore` への追加

    ```.gitignore
    node_modules
    dist/*
    ```

1. redocly の実行

    ```bash
    # OpenAPI.yaml のlinterをかける
    $ npm run bff:lint

    # 複数分割されたyamlを1つのOpenAPI.yamlにまとめる
    $ npm run bff:bundle

    # API仕様書.html として出力する
    $ npm run bff:docs
    ```

## 開発環境

- vscode + devcontainer

### ローカル端末にインストール

- Docker Desktop
- vscode
