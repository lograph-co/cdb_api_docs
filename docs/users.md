# `/users` - ユーザー

* コールデータバンクのユーザーに関する情報の操作を行います。

### `GET`

ユーザーコレクションを返す

* このリソースは、条件に一致するユーザーに関する情報を返します。
* このリソースは、要求しているユーザーの役割が `admin` でも `manager` でもない場合に `403` を返します。

#### リクエストの JSON-Schema

* `sid` (Service consumer ID) は `1` (数値)となっております。

```json
{
	"parameters": {
		"userId": {
			"type": "integer"
		},
		"sid": {
			"type": "integer"
		},
		"page": {
			"type": "integer",
			"default": "1"
		},
		"limit": {
			"type": "integer",
			"default": "100"
		}
	}
}
```
