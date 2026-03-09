# `/audiences/web` - ウェブチャネルの訪問者ログ

### `GET`

訪問者ログ

* 指定した `beginTimestamp` と `endTimestamp` の間に `campaignId` 上で観測された訪問者を返します。
* HTTPリクエストのヘッダに `Accept: text/csv` がある場合、レスポンスボディは CSV 形式になります。
* またその場合 `page` パラメータは無視されます。

#### リクエストの JSON-Schema

```json
{
    "$schema": "http://json-schema.org/draft-07/schema#",
    "$id": "jsonValidate/audiences/get.json",
    "type": "object",
    "title": "A schema for request body when `GET /audiences`.",
    "required": [
        "campaignId",
        "beginTimestamp",
        "endTimestamp"
    ],
    "properties": {
        "campaignId": {
            "title": "Campaign ID",
            "oneOf": [
                {
                    "type": "string",
                    "pattern": "^[0-9]+"
                },
                {
                    "type": "integer"
                }
            ]
        },
        "beginTimestamp": {
            "title": "Timestamp of an extraction target scope begin date.",
            "oneOf": [
                {
                    "type": "string",
                    "pattern": "^[0-9]+"
                },
                {
                    "type": "integer"
                }
            ]
        },
        "endTimestamp": {
            "title": "Timestamp of an extraction target scope end date.",
            "oneOf": [
                {
                    "type": "string",
                    "pattern": "^[0-9]+"
                },
                {
                    "type": "integer"
                }
            ]
        },
        "page": {
            "title": "Number of page",
            "oneOf": [
                {
                    "type": "string",
                    "default": "1",
                    "pattern": "^[0-9]+"
                },
                {
                    "type": "null"
                },
                {
                    "type": "integer"
                }
            ]
        },
        "limit": {
            "title": "Number of contain in 1 page",
            "oneOf": [
                {
                    "type": "string",
                    "default": "100",
                    "pattern": "^[0-9]+"
                },
                {
                    "type": "null"
                },
                {
                    "type": "integer"
                }
            ]
        },
        "sort": {
            "title": "Sort order",
            "default": "desc",
            "oneOf": [
                {
                    "type": "string",
                    "enum": [
                        "desc",
                        "asc"
                    ]
                },
                {
                    "type": "null"
                }
            ]
        },
        "sortBy": {
            "title": "Column name for sort",
            "default": "createdAt"
        }
    }
}
```

#### リクエスト項目

| 項目 | タイプ | 説明 | パターン/列挙型 | 初期値 | 必須 |
|------|:------:|------|:--------------:|:------:|:---:|
| `campaignId` | string / integer | キャンペーンID | `^[0-9]+` | | * |
| `beginTimestamp` | string / integer | 取得対象範囲の開始日時Timestamp(UTC) | `^[0-9]+` | | * |
| `endTimestamp` | string / integer | 取得対象範囲の終了日時Timestamp(UTC) | `^[0-9]+` | | * |
| `page` | string / integer / null | 取得するページ番号 | `^[0-9]+` | `1` | |
| `limit` | string / integer / null | 1ページあたりの取得件数 | `^[0-9]+` | `100` | |
| `sort` | string / null | ソート順 | `desc`, `asc` | `desc` | |
| `sortBy` | string | ソート対象カラム | | `createdAt` | |

#### レスポンスの JSON-Schema

```json
{
    "$schema": "http://json-schema.org/draft-07/schema#",
    "$id": "jsonSchema/audiences/web/get.json",
    "type": "object",
    "title": "A schema for response body when request `GET /audiences/web`.",
    "required": [
        "count",
        "total",
        "_embedded"
    ],
    "properties": {
        "count": {
            "title": "Number of audience in a response.",
            "type": "number"
        },
        "total": {
            "title": "Number of audience in a query.",
            "type": "number"
        },
        "_embedded": {
            "type": "object",
            "required": [
                "audiences"
            ],
            "properties": {
                "audiences": {
                    "title": "Audience collection",
                    "type": "array",
                    "items": {
                        "title": "Audience",
                        "type": "object",
                        "properties": {
                            "visitedAt": {
                                "title": "Visited datetime",
                                "type": "string",
                                "format": "date-time"
                            },
                            "audienceId": {
                                "title": "Audience ID",
                                "type": "string",
                                "pattern": "^[a-z0-9]{24}$"
                            },
                            "callId": {
                                "title": "Call ID",
                                "type": "string"
                            },
                            "history": {
                                "title": "Behavior history",
                                "type": "array",
                                "items": {
                                    "type": "object",
                                    "properties": {
                                        "behaviorId": {
                                            "title": "Behavior ID",
                                            "type": "string",
                                            "pattern": "^[a-z0-9]{24}$"
                                        },
                                        "createdAt": {
                                            "title": "Created datetime",
                                            "type": "string",
                                            "format": "date-time"
                                        },
                                        "url": {
                                            "title": "URL",
                                            "type": "string"
                                        },
                                        "referrer": {
                                            "title": "Referrer",
                                            "anyOf": [
                                                {
                                                    "type": "string"
                                                },
                                                {
                                                    "type": "null"
                                                }
                                            ]
                                        }
                                    }
                                }
                            },
                            "media": {
                                "title": "Media type",
                                "description": "`utm_medium` in URL of a landing page.",
                                "anyOf": [
                                    {
                                        "type": "string",
                                        "examples": [
                                            "PPC"
                                        ]
                                    },
                                    {
                                        "type": "null"
                                    }
                                ]
                            },
                            "source": {
                                "title": "Source type",
                                "description": "`utm_source` in URL of a landing page.",
                                "anyOf": [
                                    {
                                        "type": "string",
                                        "examples": [
                                            "Google"
                                        ]
                                    },
                                    {
                                        "type": "null"
                                    }
                                ]
                            },
                            "keyword": {
                                "title": "Keyword",
                                "anyOf": [
                                    {
                                        "type": "string"
                                    },
                                    {
                                        "type": "null"
                                    }
                                ]
                            },
                            "webCv": {
                                "title": "Web conversion",
                                "type": "number"
                            },
                            "callCv": {
                                "title": "Call conversion",
                                "type": "number"
                            },
                            "stayDuration": {
                                "title": "Stay duration (second)",
                                "type": "number"
                            },
                            "callDuration": {
                                "title": "Call duration (second)",
                                "anyOf": [
                                    {
                                        "type": "number"
                                    },
                                    {
                                        "type": "null"
                                    }
                                ]
                            },
                            "callerPhoneNumber": {
                                "title": "Caller phone number",
                                "anyOf": [
                                    {
                                        "type": "string"
                                    },
                                    {
                                        "type": "null"
                                    }
                                ]
                            },
                            "matchType": {
                                "title": "Match type",
                                "type": [
                                    "null",
                                    "string"
                                ]
                            },
                            "referrerDomain": {
                                "title": "Domain of referrer",
                                "type": [
                                    "null",
                                    "string"
                                ]
                            }
                        }
                    }
                }
            }
        }
    }
}
```

#### レスポンス項目

| 項目 | タイプ | 説明 | 備考 |
|-----|-------|------|-----|
| `count` | number | カウント | レスポンスに含まれている訪問者ログ数 |
| `total` | number | トータル | Query結果に含まれている訪問者ログ数 |

##### `"_embedded.audiences"` 以下

| 項目 | タイプ | 説明 | 備考 |
|-----|-------|------|-----|
| `visitedAt` | date | 来訪日時 |  |
| `audienceId` | string | 来訪者ID |  |
| `media` | string | 広告種別 | `utm_medium` の値 |
| `source` | string | 広告媒体 | `utm_source` の値 |
| `content` | string | 広告コンテンツ | `utm_content` の値 |
| `keyword` | string | キーワード |  |
| `history` | object | 来訪履歴ログ |  |
| `history.behaviorId` | string | ログID |
| `history.createdAt` | date | 作成日 |
| `history.url` | string | URL |
| `history.referrer` | string | リファラ |
| `webCv` | number | ウェブCV |  |
| `callCv` | number | 電話CV |  |
| `stayDuration` | number | 滞在時間 | 秒 |
| `callDuration` | number | 通話時間 | 秒 |
| `callerPhoneNumber` | string | 発信者番号 |  |
| `matchType` | string | マッチタイプ | 広告表示につながったマッチタイプ |
| `referrerDomain` | string | リファラドメイン |  |
| `landingPageUrl` | string | ランディングページURL |  |
