# `/behaviors/phone/calls` - 入電ログ

### `GET`

コールログ

* 指定した `beginTimestamp` と `endTimestamp` の間に `campaignId` 上で観測された入電を返します。
* HTTPリクエストのヘッダに `Accept: text/csv` がある場合、レスポンスボディは CSV 形式になります。
* またその場合 `page` パラメータは無視されます。

#### リクエストの JSON-Schema

```json
{
	"$schema": "http://json-schema.org/draft-07/schema#",
	"$id": "jsonValidate/behaviors/phone/get.json",
	"type": "object",
	"title": "A schema for request parameters when `GET /behaviors/phone`.",
	"required": [
		"campaignId",
		"beginTimestamp",
		"endTimestamp"
	],
	"properties": {
		"campaignId": {
			"title": "Campaign ID",
			"type": "string",
			"pattern": "^[0-9]+"
		},
		"observersId": {
			"title": "Observer ID",
			"type": "string",
			"pattern": "^[0-9,]+"
		},
		"beginTimestamp": {
			"title": "Timestamp of an extraction target scope begin date.",
			"type": "string",
			"pattern": "^[0-9]+"
		},
		"endTimestamp": {
			"title": "Timestamp of an extraction target scope end date.",
			"type": "string",
			"pattern": "^[0-9]+"
		},
		"page": {
			"title": "Number of page",
			"anyOf": [
				{
					"type": "string",
					"default": "1",
					"pattern": "^[0-9]+"
				},
				{
					"type": "null"
				}
			]
		},
		"limit": {
			"title": "Number of contain in 1 page",
			"anyOf": [
				{
					"type": "string",
					"default": "100",
					"pattern": "^[0-9]+"
				},
				{
					"type": "null"
				}
			]
		},
		"sort": {
			"title": "Sort order",
			"default": "desc",
			"anyOf": [
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

#### レスポンスの JSON-Schema

```json
{
	"$schema": "http://json-schema.org/draft-07/schema#",
	"$id": "jsonSchema/behaviors/phone/get.json",
	"type": "object",
	"title": "A schema for response body when request `GET /behaviors/phone/calls`.",
	"required": [
		"count",
		"total",
		"_embedded"
	],
	"properties": {
		"count": {
			"title": "Number of audiences in a response.",
			"type": "number"
		},
		"total": {
			"title": "Number of audiences in a query.",
			"type": "number"
		},
		"_embedded": {
			"type": "object",
			"properties": {
				"calls": {
					"type": "array",
					"items": {
						"type": "object"
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
| `count` | number | カウント | レスポンスに含まれている入電ログ数 |
| `total` | number | トータル | Query結果に含まれている入電ログ数 |

##### `"_embedded.calls"` 以下

* 以下項目は利用者及び設定状況に応じて変化する可能性があります。

| 項目 | タイプ | 説明 | 備考 |
|-----|-------|------|-----|
| `visitedAt` | date | 来訪日時 |  |
| `calledAt` | date | 入電日時 |  |
| `observerId` | number | 観測点ID |  |
| `observerLabel` | string | 観測点名 |  |
| `audienceId` | string | 来訪者ID |  |
| `callId` | string | 入電ID |  |
| `media` | string | 広告種別 | `utm_medium` の値 |
| `source` | string | 広告媒体 | `utm_source` の値 |
| `content` | string | 広告コンテンツ | `utm_content` の値 |
| `keyword` | string | キーワード |  |
| `pageView` | number | PV |  |
| `callDuration` | number | 通話時間 | 秒 |
| `callerPhoneNumber` | string | 発信者番号 |  |
| `trackingPhoneNumber` | string | 計測番号 |  |
| `hangupCode` | number | 切断理由コード |  |
| `deviceType` | string | 端末種別 |  |
| `causedUrl` | string | ランディングページ |  |
| `recordedAudioUrl` | string | 録音ファイル |  |
| `matchType` | string | マッチタイプ | 広告表示につながったマッチタイプ |
| `referrerDomain` | string | リファラドメイン |  |
