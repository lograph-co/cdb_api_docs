# `/trackingNumbers` - 計測番号

* コールデータバンク内の計測設定構造のうち、計測番号を表します。
* 計測番号は `campaignId` と `observers` を持ち、それぞれえ束縛先のキャンペーンおよび観測点を示します。
* 転送先電話番号は現在未使用です。

### `GET`

計測番号コレクションを返す。

#### リクエストの JSON-Schema

```json
{
	"$schema": "http://json-schema.org/draft-07/schema#",
	"$id": "jsonValidate/trackingNumbers/get.json",
	"type": "object",
	"title": "A schema for request parameters when `GET /trackingNumbers`.",
	"properties": {
		"trackingNumberId": {
			"title": "Tracking number ID",
			"anyOf": [
				{
					"type": "string",
					"pattern": "^[0-9]+$"
				},
				{
					"type": "number"
				},
				{
					"type": "null"
				}
			]
		},
		"accountId": {
			"title": "Account ID",
			"anyOf": [
				{
					"type": "string",
					"pattern": "^[0-9]+$"
				},
				{
					"type": "number"
				},
				{
					"type": "null"
				}
			]
		},
		"campaignId": {
			"title": "Campaign ID",
			"anyOf": [
				{
					"type": "string",
					"pattern": "^[0-9]+$"
				},
				{
					"type": "number"
				},
				{
					"type": "null"
				}
			]
		},
		"observerId": {
			"title": "Observer ID",
			"anyOf": [
				{
					"type": "string",
					"pattern": "^[0-9]+$"
				},
				{
					"type": "number"
				},
				{
					"type": "null"
				}
			]
		},
		"isBinding": {
			"title": "Is binding",
			"anyOf": [
				{
					"type": "string",
					"pattern": "^[01]$"
				},
				{
					"type": "number"
				},
				{
					"type": "null"
				}
			]
		},
		"phoneNumber": {
			"title": "Phone number",
			"anyOf": [
				{
					"type": "string",
					"pattern": "^[0-9]+$"
				},
				{
					"type": "null"
				}
			]
		},
		"page": {
			"title": "Number of page",
			"anyOf": [
				{
					"type": "string",
					"pattern": "^[0-9]+$"
				},
				{
					"type": "number"
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
					"pattern": "^[0-9]+$"
				},
				{
					"type": "number"
				},
				{
					"type": "null"
				}
			]
		}
	}
}
```

#### レスポンスの JSON-Schema

```json
{
	"$schema": "http://json-schema.org/draft-07/schema#",
	"$id": "jsonSchema/trackingNumbers/get.json",
	"type": "object",
	"title": "A schema for response body when request `GET /trackingNumbers`.",
	"required": [
		"count",
		"total",
		"_embedded"
	],
	"properties": {
		"count": {
			"title": "Number of campaign in a response.",
			"type": "number"
		},
		"total": {
			"title": "Number of campaign in a query.",
			"type": "number"
		},
		"_embedded": {
			"type": "object",
			"required": [
				"trackingNumbers"
			],
			"properties": {
				"trackingNumbers": {
					"title": "Tracking number collection",
					"type": "array",
					"items": {
						"title": "Tracking number",
						"type": "object",
						"properties": {
							"trackingNumberSupplierId": {
								"description": "計測番号提供者 ID",
								"bsonType": "int"
							},
							"phoneNumber": {
								"description": "電話番号",
								"bsonType": "string"
							},
							"forwardingPhoneNumber": {
								"description": "転送先電話番号",
								"bsonType": "string"
							},
							"accountId": {
								"description": "保有元アカウント ID",
								"bsonType": "int"
							},
							"campaignId": {
								"description": "束縛先キャンペーン ID",
								"bsonType": "int"
							},
							"observers": {
								"description": "束縛先観測点 ID",
								"bsonType": "array",
								"items": {
									"bsonType": "int"
								}
							},
							"option": {
								"description": "オプション",
								"bsonType": "object",
								"oneOf": [
									{
										"description": "コールノート",
										"required": [
											"clientCd"
										],
										"properties": {
											"name": {
												"description": "クライアント名",
												"bsonType": "string"
											},
											"clientCd": {
												"description": "クライアント CD",
												"bsonType": "string"
											},
											"attachCd": {
												"description": "アタッチ CD",
												"bsonType": "string"
											},
											"clientMail": {
												"description": "通話ログ通知メールアドレス",
												"bsonType": "string"
											},
											"clientCcyPushMail": {
												"description": "プッシュ通知メールアドレス",
												"bsonType": "string"
											},
											"clientWebUse": {
												"description": "Web 通知パラメータ",
												"bsonType": "bool"
											},
											"clientSmsUse": {
												"description": "SMS 通知パラメータ",
												"bsonType": "bool"
											},
											"customerNumberDisp": {
												"description": "番号透過設定パラメータ",
												"bsonType": "bool"
											},
											"clientGuidanceFile": {
												"description": "個別ガイダンス設定パラメータ",
												"bsonType": "string"
											}
										}
									}
								]
							},
							"createdAt": {
								"description": "作成日時",
								"bsonType": "date"
							},
							"deletedAt": {
								"description": "削除日時",
								"bsonType": "date"
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
| `count` | number | カウント | レスポンスに含まれている計測番号数 |
| `total` | number | トータル | Query結果に含まれている計測番号数 |
| `createdAt` | date | 作成日時 |  |

##### `"_embedded.trackingNumbers"` 以下

| 項目 | タイプ | 説明 | 備考 |
|-----|-------|------|-----|
| `trackingNumberSupplierId` | number | 計測番号ID |  |
| `phoneNumber` | string | 電話番号 |  |
| `accountId` | number | アカウントID |  |
| `campaignId` | number | キャンペーンID |  |
| `observers` | array | 観測点 |  |
