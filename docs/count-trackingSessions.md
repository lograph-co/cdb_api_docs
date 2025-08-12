# `/count/trackingSessions` - 計測セッション

### `GET`

日別計測セッション数を集計する

* 任意のキャンペーン上で観測された計測セッションを計測チャネルと期間を指定して集計します。
* 指定可能な期間は2018年1月1日からリクエスト当日まで、最長31日間です。
* 2018年1月1日から1月8日までを集計範囲として指定した場合、1月1日0時から1月9日0時未満に開始した計測セッションを集計の対象とします。
* 一日だけ集計する場合は `$beginDate` と `$endDate` には同じ日付を指定します。
* 集計は観測点別に行われますが、集計結果には指定したキャンペーン全体での総数も含まれます。
* 集計された計測セッションが0の日も集計結果に含まれます。

#### リクエストの JSON-Schema

```json
{
	"$schema": "http://json-schema.org/draft-07/schema#",
	"$id": "jsonValidate/count/trackingSessions/get.json",
	"type": "object",
	"title": "A schema for request parameters of `GET /count/trackingSessions`.",
	"required": [
		"campaignId",
		"beginDate",
		"endDate",
		"channel"
	],
	"properties": {
		"campaignId": {
			"title": "集計対象となるキャンペーン ID",
			"type": "number"
		},
		"beginDate": {
			"title": "集計範囲の開始日",
			"type": "string",
			"format": "date",
			"example": "2018-01-01"
		},
		"endDate": {
			"title": "集計範囲の終了日",
			"type": "string",
			"format": "date",
			"example": "2018-01-01"
		},
		"channel": {
			"title": "集計対象となるチャネル",
			"type": "string",
			"default": "phone",
			"enum": [
				"phone",
				"web"
			]
		}
	}
}
```

#### レスポンスの JSON-Schema

```json
{
	"$schema": "http://json-schema.org/draft-07/schema#",
	"$id": "jsonSchema/count/trackingSessions/get.json",
	"type": "object",
	"title": "A schema for response body when `GET /count/trackingSessions`.",
	"required": [
		"trackingSessions"
	],
	"properties": {
		"trackingSessions": {
			"title": "計測セッションの集計結果",
			"type": "object",
			"properties": {
				"byCampaign": {
					"title": "キャンペーン別集計結果",
					"type": "array",
					"uniqueItems": true,
					"items": {
						"type": "object",
						"properties": {
							"campaignId": {
								"title": "キャンペーン ID",
								"type": "number"
							},
							"label": {
								"title": "表示名",
								"type": "string"
							},
							"counts": {
								"title": "日別集計結果",
								"type": "array",
								"uniqueItems": true,
								"properties": {
									"date": {
										"title": "日付",
										"type": "string",
										"format": "date"
									},
									"count": {
										"title": "計測セッション数",
										"type": "number"
									}
								}
							}
						}
					}
				},
				"byObserver": {
					"title": "観測点別集計結果",
					"type": "array",
					"uniqueItems": true,
					"items": {
						"type": "object",
						"properties": {
							"observerId": {
								"title": "観測点 ID",
								"type": "number"
							},
							"label": {
								"title": "表示名",
								"type": "string"
							},
							"counts": {
								"title": "日別集計結果",
								"type": "array",
								"uniqueItems": true,
								"properties": {
									"date": {
										"title": "日付",
										"type": "string",
										"format": "date"
									},
									"count": {
										"title": "計測セッション数",
										"type": "number"
									}
								}
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
| `count` | number | カウント | レスポンスに含まれているセッション数 |
| `total` | number | トータル | Query結果に含まれているセッション数 |

##### `"trackingSessions.byCampaign"` 以下

| 項目 | タイプ | 説明 | 備考 |
|-----|-------|------|-----|
| `campaignId` | number | キャンペーンID |  |
| `label` | string | キャンペーン名 |  |
| `counts.date` | string | セッション集計日 | `yyyy-mm-dd` |
| `counts.counr` | number | セッション数 |  |
