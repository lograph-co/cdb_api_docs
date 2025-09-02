# `/accounts` - アカウント

* コールデータバンク内の計測設定構造へのアクセスポイントとなります。
* コールデータバンク内の計測設定構造の最上位に位置します。
* アカウントには所有者となるユーザーが必ず一つ存在します。
* それとは別にアクセス権を持ったユーザー（所属ユーザーといいます）を無制限に設定することが出来ます。
* アカウントは代理店という性質を持つことが出来ます。
* そのような性質を持ったアカウントを代理店アカウントといいます。
* 代理店アカウントの所有者ないし所属ユーザーは自らの代理店アカウントを代理店してアカウント付きのユーザーを作成することができます。
* この場合、作成された側のアカウントはクライアントアカウントといいます。
* アカウントは複数のキャンペーンにひも付きます。

### 代理店

アカウントの `isAgent`という属性が `true` の場合、所有者またはアカウントに所属するユーザーは *"代理店"* となり、そのアカウントは *"代理店"* または *"代理店アカウント"* と呼ばれます。
代理店が新しいアカウントを作成するとき、新しいアカウントの `agencyId` に代理店のアカウント ID が挿入されます。

### `GET`

アカウントコレクションを返す

* `accountId` が指定されている場合、それ以外のパラメータは無視されます。
* `relationshipTypes` に指定されている関係性にマッチするアカウントを抽出します。
* `relationshipTypes` には複数の値をカンマ区切りで指定することができます。
* `relationshipTypes` に複数の値を指定している場合、OR として評価されます。

#### `relationshipTypes` に指定可能な値

| 値 | 意味 |
| --- | --- |
| `owner` | 自分が所有しているアカウント |
| `belongs` | 自分が所属しているアカウント |
| `client` | 自分が所属する代理店アカウントが代理店になっているアカウント |
| `agency` | 自分が所属する代理店アカウント |

#### リクエストの JSON-Schema

```json
{
	"$schema": "http://json-schema.org/draft-07/schema#",
	"$id": "jsonValidate/accounts/get.json",
	"type": "object",
	"title": "A schema for request parameters when `GET /accounts`.",
	"properties": {
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
		"relationshipTypes": {
			"title": "Relationship Types",
			"anyOf": [
				{
					"type": "string",
					"default": "owner,belongs"
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
	"$id": "jsonSchema/accounts/get.json",
	"type": "object",
	"title": "A schema for response body when request `GET /accounts`.",
	"required": [
		"count",
		"total",
		"_embedded"
	],
	"properties": {
		"count": {
			"title": "Number of account in a response.",
			"type": "number"
		},
		"total": {
			"title": "Number of account in a query.",
			"type": "number"
		},
		"_embedded": {
			"type": "object",
			"required": [
				"accounts"
			],
			"properties": {
				"accounts": {
					"title": "Account collection",
					"type": "array",
					"items": {
						"title": "Account",
						"type": "object",
						"required": [
							"accountId",
							"label",
							"ownerId",
							"createdAt"
						],
						"properties": {
							"accountId": {
								"title": "Account ID",
								"type": "number",
								"readOnly": true,
								"examples": [
									1
								]
							},
							"label": {
								"title": "Account name",
								"type": "string",
								"examples": [
									"Account name"
								]
							},
							"ownerId": {
								"title": "Owner user ID",
								"type": "number",
								"examples": [
									1
								]
							},
							"agencyId": {
								"title": "Agency account ID",
								"type": "number",
								"examples": [
									1
								]
							},
							"belongs": {
								"type": "array",
								"items": {
									"type": "object",
									"properties": {
										"userId": {
											"title": "Belonged user ID",
											"type": "number",
											"examples": [
												1
											]
										}
									}
								}
							},
							"isAgency": {
								"title": "Whether account is agency",
								"type": "boolean",
								"default": false
							},
							"createdAt": {
								"title": "Created datetime",
								"type": "string",
								"format": "date-time",
								"readOnly": true
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
| `count` | number | カウント | レスポンスに含まれているアカウント数 |
| `total` | number | トータル | Query結果に含まれているアカウント数 |


##### `"properties._embedded"` 以下

| 項目 | タイプ | 説明 | 備考 |
|-----|-------|------|-----|
| `accountId` | number | アカウントID |  |
| `serviceConsumerId` | number | サービスコンシューマーID | `1` 固定 |
| `ownerId` | number | オーナーID |  |
| `label` | string | アカウント名 |  |
| `isAgency` | boolean | 代理店フラグ |  |
| `agencyId` | number | 代理店アカウントID |  |
| `createdAt` | date | 作成日 |  |
| `belongs` | array | belongs | 参照できるユーザーを `userId` で指定 |

##### `"properties._embedded.campaigns"` 以下

| 項目 | タイプ | 説明 |
|-----|-------|------|
| `campaignId` | number | キャンペーンID |
| `accountId` | number | アカウントID |
| `label` | string | キャンペーン名 |
| `belongs` | array | 参照できるユーザーを `userId` で指定 |  |
| `createdAt` | date | キャンペーン作成日 |
