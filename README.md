![logo](docs/assets/logo.png)
# コールデータバンク API Documents

## what

コールデータバンク用のAPIドキュメントをMarkdownでHostするもの。

## requirements

* node/npm
* [docsify](https://yamachan.github.io/docsify-docs-ja/#/)
    * `npm i docsify-cli -g`

## how

### serve

このリポジトリをClone後、cdして以下コマンド実行とすると `http://localhost:3000/` でプレビューできます。

```shell
docsify serve docs
```

### where

以下のような構造になっていますので `docs` ディレクトリ配下の `.md` ファイルを修正してください。また、 `README.md` ファイルが index となります。

```
├── docs
│   ├── accounts.md
│   ├── audiences-web.md
│   ├── authentications.md
│   ├── behaviors-phone-calls.md
│   ├── campaigns.md
│   ├── count-trackingSessions.md
│   ├── index.html
│   ├── me.md
│   ├── observers.md
│   ├── README.md
│   ├── serviceConsumers.md
│   ├── trackingNumbers.md
│   └── users.md
└── README.md
```

### deploy

修正後、修正したファイルを `commit` して `main` ブランチに `push` すると以下URLに `deploy` されます。

https://docs.omni-databank.com/
