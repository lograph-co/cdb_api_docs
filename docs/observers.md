# `/observers` - 観測点

### `GET`

観測点コレクションを返す。

観測点IDが指定された場合は、指定された観測点IDに一致する1件分のコレクションを返す(他のパラメータは無視される)。キャンペーンIDが指定された場合は、指定されたキャンペーンIDにひもづく観測点のコレクションを返す。観測点ID、キャンペーンIDがいずれも指定されない場合はエラーを返す。

* 観測点は訪問者の行動を計測します。
* 訪問者の行動や訪問時の状況が発動条件を満たすと観測点は計測を開始します。
* 観測点に設定された発動条件を全て満たさなければ計測は開始しません。
* 観測点毎に異なる発動条件を設定することができます。
* 観測点が計測を開始すると計測セッションが作られます。
* 計測セッションには有効期限があり、有効期限内に新たな行動が計測されない場合、計測セッションは終了します。
* 発動条件に指定できる内容は計測するチャネルによって異なります。
* 1つのチャネルに対して複数の観測点を作成することができます。
* ウェブチャネルのみ観測点を複数作成することはできません。
* 複数の観測点によって複数の計測セッションが同時に開始することがあります。
* 電話チャネルの観測点は必ず一つの置換対象番号に対してつくられます。
* 電話チャネルの観測点には計測用の電話番号が指定でき、これを計測番号と呼びます。
* 計測番号が観測点に指定されることを、束縛すると言います。
* 電話チャネルで開始した計測セッションには計測番号が割り当てられます。
* 計測番号に入電があった際、その計測番号が割り当てられていて計測中の計測セッション中の行動として解釈されます。
* 電話チャネルの観測点に1つだけ計測番号が束縛されている場合、全ての計測セッションでその計測番号が使用されます。
* 複数の計測番号が束縛されている場合、計測セッション毎にランダムに異なる計測番号が使用されます。
* 使用可能な計測番号がない場合に代わりに使用される電話番号をデフォルト番号として設定することができます。
* 異なる観測点に同一の計測番号を束縛させる事ができます。
* 計測番号はキャンペーンにも束縛させる事ができます。
* キャンペーンと観測点の両方に計測番号が束縛されている場合、キャンペーンの計測番号が優先的に使用されます。
* 1つのキャンペーン中で一度でも入電のあった訪問者に対しては、以降毎回必ず同じ計測番号が使用されます。

#### リクエストの JSON-Schema

```json
{
	"$schema": "http://json-schema.org/draft-07/schema#",
	"$id": "jsonValidate/observers/get.json",
	"type": "object",
	"title": "A schema for request parameters when `GET /observers`.",
	"properties": {
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
	"$id": "jsonSchema/observers/get.json",
	"type": "object",
	"title": "A schema for response body when request `GET /observers`.",
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
				"observers"
			],
			"properties": {
				"observers": {
					"title": "Observer collection",
					"type": "array",
					"items": {
						"title": "Observer",
						"type": "object",
						"properties": {
							"campaignId": {
								"title": "観測点に紐づくキャンペーン ID",
								"type": "number"
							},
							"label": {
								"title": "表示名",
								"type": "string"
							},
							"lifetime": {
								"title": "計測セッションの継続期間（ミリ秒）",
								"type": "number"
							},
							"priority": {
								"title": "観測点の優先順位（大きな方を優先）",
								"type": "number"
							},
							"condition": {
								"type": "object",
								"oneOf": [
									{
										"required": [
											"channel",
											"targetDevice"
										],
										"properties": {
											"channel": {
												"title": "対象チャネル",
												"type": "string",
												"enum": [
													"phone"
												]
											},
											"targetPhoneNumber": {
												"title": "置換対象番号",
												"type": "string"
											},
											"defaultTrackingNumberId": {
												"title": "デフォルト番号",
												"type": "number"
											},
											"targetDevice": {
												"title": "対応デバイス",
												"type": "string",
												"enum": [
													"all",
													"pc",
													"mobile"
												]
											},
											"triggers": {
												"title": "観測点の発動条件トリガ",
												"type": "array",
												"uniqueItems": true,
												"items": {
													"type": "object",
													"required": [
														"target",
														"type",
														"method",
														"formula"
													],
													"properties": {
														"target": {
															"title": "対象",
															"type": "string",
															"enum": [
																"landing",
																"referrer",
																"recent"
															]
														},
														"type": {
															"title": "トリガ種別",
															"type": "string",
															"enum": [
																"domain",
																"path",
																"query"
															]
														},
														"method": {
															"title": "トリガの評価方法",
															"type": "string",
															"enum": [
																"equal",
																"forwardMatch",
																"partialMatch",
																"regex"
															]
														},
														"formula": {
															"title": "トリガの評価式",
															"type": "string"
														},
														"subject": {
															"title": "トリガの評価対象（パラメータ名）",
															"type": "string"
														}
													}
												}
											}
										}
									},
									{
										"required": [
											"channel"
										],
										"properties": {
											"channel": {
												"title": "対象チャネル",
												"type": "string",
												"enum": [
													"web"
												]
											}
										}
									}
								]
							},
							"createdAt": {
								"title": "作成日時",
								"type": "string",
								"format": "date-time"
							},
							"deletedAt": {
								"title": "削除日時",
								"type": "string",
								"format": "date-time"
							},
							"_embedded": {
								"type": "object",
								"properties": {
									"trackingNumbers": {
										"title": "Tracking number collection",
										"type": "array",
										"items": {
											"title": "Tracking number",
											"type": "object",
											"properties": {
												"trackingNumberId": {
													"title": "Tracking number ID",
													"type": "number"
												},
												"phoneNumber": {
													"title": "電話番号",
													"type": "string"
												},
												"observers": {
													"title": "束縛先観測点 ID collection",
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
| `count` | number | カウント | レスポンスに含まれている観測点数 |
| `total` | number | トータル | Query結果に含まれている観測点数 |

##### `"_embedded.observers"` 以下

| 項目 | タイプ | 説明 | 備考 |
|-----|-------|------|-----|
| `observerId` | number | 観測点ID |  |
| `campaignId` | number | キャンペーンID |  |
| `label` | string | キャンペーン名 |  |
| `lifetime` | number | 有効期限 |  |
| `priority` | number | 優先度 |  |
| `createdAt` | date | 作成日時 |  |
| `belongs` | array | 参照できるユーザーを `userId` で指定 |  |
| `condition.channel` | string | 計測チャネル | `web` または `phone` |
| `condition.targetPhoneNumber` | string | 置換対象番号 |  |
| `condition.targetDevice` | string | 対応デバイス | `pc` , `mobile` 、またはその両方である `all` |
| `belongs` | array | belongs | 参照できるユーザーを `userId` で指定 |
| `condition.triggers` | array | 発動条件トリガ |  |

##### `"condition.triggers"` 以下

* webチャネルの場合、 `condition.triggers` は存在しません。

| 項目 | タイプ | 説明 | 備考 |
|-----|-------|------|-----|
| `target` | string | 置換対象URL種別 | 評価の対象となるURL設定。 ランディングページ（ `landing` ）、リファラ（ `referrer` ）、最近見たページ（ `currentUrl` ） から選択 |
| `type` | string | 評価対象要素 | URLのどの箇所を評価対象とするかを指定。メイン（ `domain` ）、パス（ `path` ）、URL パラメータ（ `query` ）から選択 |
| `subject` | string | パラメータ名 | 評価対象となるパラメータ名 |
| `method` | string | 評価方法 | 完全一致（ `equal` ）、前方一致（ `forwardMatch` ）、部分一致（ `partialMatch` ）、正規表現（ `regex` ）から選択  |
| `formula` | string | 評価式 | 評価文字列 |
