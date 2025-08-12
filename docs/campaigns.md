# `/campaigns` - キャンペーン

* コールデータバンク内の計測設定構造のうち、観測したデータを一意に扱う設定単位です。
* キャンペーンは1つのアカウントにひも付き、複数の観測点にひも付きます。

### `GET`

キャンペーンコレクションを返す

* `campaignId` が指定されている場合、それ以外のパラメータは無視されます。

#### リクエストの JSON-Schema

```json
{
	"$schema": "http://json-schema.org/draft-07/schema#",
	"$id": "jsonValidate/campaigns/get.json",
	"type": "object",
	"title": "A schema for request parameters when `GET /campaigns`.",
	"properties": {
		"campaignId": {
			"title": "Campaign ID",
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
		"accountId": {
			"title": "Account ID",
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
			"title": "Number Of Page",
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
		"limit": {
			"title": "Number Of Contain in 1 Page",
			"anyOf": [
				{
					"type": "string",
					"pattern": "^[0-9]+$"
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
	"$id": "jsonSchema/campaigns/get.json",
	"type": "object",
	"title": "A schema for response body when request `GET /campaigns`.",
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
				"campaigns"
			],
			"properties": {
				"campaigns": {
					"title": "Campaign collection",
					"type": "array",
					"items": {
						"title": "Campaign",
						"type": "object",
						"required": [],
						"properties": {
							"campaignId": {
								"title": "Campaign ID",
								"type": "number"
							},
							"accountId": {
								"title": "Account ID",
								"type": "number"
							},
							"label": {
								"title": "Campaign name",
								"type": "string"
							},
							"defaultTrackingNumberId": {
								"title": "Default tracking number ID",
								"anyOf": [
									{
										"type": "number"
									},
									{
										"type": "null"
									}
								]
							},
							"beganAt": {
								"title": "Begin date of the campaign",
								"anyOf": [
									{
										"type": "string",
										"format": "date-time"
									},
									{
										"type": "null"
									}
								]
							},
							"finishedAt": {
								"title": "Finish date of the campaign",
								"anyOf": [
									{
										"type": "string",
										"format": "date-time"
									},
									{
										"type": "null"
									}
								]
							},
							"createdAt": {
								"title": "Created datetime",
								"type": "string",
								"format": "date-time"
							},
							"deletedAt": {
								"title": "Deleted datetime",
								"anyOf": [
									{
										"type": "string",
										"format": "date-time"
									},
									{
										"type": "null"
									}
								]
							},
							"attributes": {
								"title": "Campaign attributes",
								"type": "array",
								"items": {
									"type": "object",
									"properties": {
										"name": {
											"title": "Attribute name",
											"type": "string"
										},
										"value": {
											"title": "JSON encoded value"
										}
									}
								}
							},
							"_embedded": {
								"type": "object",
								"properties": {
									"observers": {
										"type": "array",
										"items": {
											"type": "object",
											"properties": {
												"observerId": {
													"title": "Observer ID",
													"type": "number"
												},
												"label": {
													"title": "Observer name",
													"type": "string"
												},
												"condition": {
													"title": "Condition",
													"type": "object",
													"properties": {
														"channel": {
															"title": "Target channel for observation",
															"type": "string",
															"enum": [
																"web",
																"phone"
															]
														}
													},
													"if": {
														"properties": {
															"channel": {
																"type": "string",
																"enum": [
																	"phone"
																]
															}
														},
														"required": [
															"channel"
														]
													},
													"then": {
														"properties": {
															"targetPhoneNumber": {
																"title": "Target phone number for replacement",
																"type": "string",
																"pattern": "^[0-9]+$"
															},
															"targetDevice": {
																"title": "For which device type replace",
																"type": "string",
																"enum": [
																	"all",
																	"pc",
																	"mobile"
																]
															}
														}
													}
												}
											}
										}
									},
									"trackingNumbers": {
										"type": "array",
										"items": {
											"type": "object",
											"properties": {
												"trackingNumberId": {
													"title": "Tracking number ID",
													"type": "number"
												},
												"phoneNumber": {
													"title": "Phone number",
													"type": "string"
												},
												"observers": {
													"type": "array",
													"items": {
														"title": "Observer ID",
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
			}
		}
	}
}
```

#### レスポンス項目

| 項目 | タイプ | 説明 | 備考 |
|-----|-------|------|-----|
| `count` | number | カウント | レスポンスに含まれているキャンペーン数 |
| `total` | number | トータル | Query結果に含まれているキャンペーン数 |

##### `"_embedded.campaigns"` 以下

| 項目 | タイプ | 説明 | 備考 |
|-----|-------|------|-----|
| `campaignId` | number | キャンペーンID |  |
| `accountId` | number | アカウントID |  |
| `label` | string | キャンペーン名 |  |
| `beganAt` | date | キャンペーン開始日時 |  |
| `finishedAt` | date | キャンペーン終了日時 |  |
| `createdAt` | date | キャンペーン作成日時 |  |
| `deletedAt` | date | キャンペーン削除日時 |  |
| `defaultTrackingNumberId` | number | デフォルト計測番号ID | 配信時、最優先される計測番号ID |
| `belongs` | array | belongs | 参照できるユーザーを `userId` で指定 |
