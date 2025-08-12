# `/authentications` - 認証

本 API の認証は [OAuth 2.0 Bearer](https://tools.ietf.org/html/rfc6750) に準拠しています。認証に使われるアクセストークンの仕様は [JSON Web Token](https://tools.ietf.org/html/rfc7519) に準拠しています。以下は JSON Web Token （以下、JWT）に使われる認証用のフィールド例です。トークンの生成にはサービスコンシューマ毎に異なる秘密鍵が使われます。

```json
{
	"aud": {
		"id": "{userId}",
		"role": "{role}",
		"label": "{label}"
	},
	"exp": "{expiresIn}"
}
```

### Role

`system`, `admin`, `manager`, `client`, `guest` の5つが役割として定義されています。そのうち`system` と `guest` は仮想的な役割なので API 上に現れることはありません。役割は `user.role` に整数値として与えられています。

| Role | Value |
| --- | --- |
| admin | 2 |
| manager | 4 |
| client | 8 |

### `POST`

認証をするリソース

#### リクエストの JSON-Schema

* `sid` (Service consumer ID) は `“1”` (文字列)となっております。

```json
{
	"$schema": "http://json-schema.org/draft-07/schema#",
	"$id": "jsonValidate/authentications/post.json",
	"type": "object",
	"title": "A schema for request body when request `POST /authentications`.",
	"required": [
		"sid",
		"email",
		"password"
	],
	"properties": {
		"sid": {
			"title": "Service consumer ID",
			"oneOf": [
				{
					"type": "number"
				},
				{
					"type": "string",
					"pattern": "^[0-9]+$"
				}
			]
		},
		"email": {
			"title": "E-mail address",
			"type": "string",
			"format": "email"
		},
		"password": {
			"title": "Password",
			"type": "string"
		}
	}
}
```

#### レスポンスの JSON-Schema

```json
{
	"$schema": "http://json-schema.org/draft-07/schema#",
	"$id": "jsonSchema/authentications/post.json",
	"type": "object",
	"title": "A schema for response body when request `POST /authentications`.",
	"required": [
		"accessToken",
		"refreshToken",
		"expiresIn"
	],
	"properties": {
		"accessToken": {
			"title": "Access token",
			"type": "string"
		},
		"refreshToken": {
			"title": "Refresh token",
			"type": "string"
		},
		"expiresIn": {
			"title": "Expiration",
			"type": "number"
		}
	}
}
```

#### レスポンス項目

| 項目 | タイプ | 説明 |
|-----|-------|------|
| `expiresIn` | number | 有効期間 |
| `accessToken` | string | アクセストークン |
| `refreshToken` | string | リフレッシュトークン |

### `PUT`

リフレッシュトークンで再認証をするリソース

#### リクエストの JSON-Schema

```json
{
	"$schema": "http://json-schema.org/draft-07/schema#",
	"$id": "jsonValidate/authentications/put.json",
	"type": "object",
	"title": "A schema for request body when request `PUT /authentications`.",
	"required": [
		"refreshToken"
	],
	"properties": {
		"refreshToken": {
			"title": "Refresh token",
			"type": "string"
		}
	}
}
```

#### レスポンスの JSON-Schema

```json
{
	"$schema": "http://json-schema.org/draft-07/schema#",
	"$id": "jsonSchema/authentications/post.json",
	"type": "object",
	"title": "A schema for response body when request `PUT /authentications`.",
	"required": [
		"accessToken",
		"refreshToken",
		"expiresIn"
	],
	"properties": {
		"accessToken": {
			"title": "Access token",
			"type": "string"
		},
		"refreshToken": {
			"title": "Refresh token",
			"type": "string"
		},
		"expiresIn": {
			"title": "Expiration",
			"type": "number"
		}
	}
}
```

#### レスポンス項目

| 項目 | タイプ | 説明 |
|-----|-------|------|
| `expiresIn` | number | 有効期間 |
| `accessToken` | string | アクセストークン |
| `refreshToken` | string | リフレッシュトークン |
