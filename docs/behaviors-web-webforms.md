# `/behaviors/web/webforms` - フォームログ

### `GET`

フォームログ

* 指定したキャンペーンのフォームログを返します。
  * 指定期間内に来訪者が閲覧し、フォーム入力内容が記録されたページが対象です。
* リクエストヘッダに `Accept: text/csv` を指定すると、レスポンスボディは CSV 形式になります。
  * CSV 形式では `page` と `limit` は無視されます。
  * キャンペーンで出力項目を個別に設定していない場合、CSVではフォームの入力値が「入力項目1」「入力項目2」…の列に分かれます。列数と並びは取得結果によって変わります。
* フォーム入力値をマスクすると、対象の値は `****` になります。
  * `needMask` が `1` の場合、または API を呼び出したユーザーが観測点のマスク対象に設定されている場合に適用されます。
  * マスクの対象は、キャンペーンで指定した項目です。指定がない場合は、すべてのフォーム入力値が対象です。

#### リクエストの JSON-Schema

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "jsonValidate/behaviors/web/get.json",
  "type": "object",
  "title": "A schema for request parameters when `GET /behaviors/web/webforms`.",
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
    "needMask": {
      "title" :"String of indicating whether to perform mask.",
      "type": "string",
      "default": "0",
      "pattern": "^[0-1]$"
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
      "default": "createdAt",
      "type": "string",
      "enum": [
        "createdAt",
        "visitedAt"
      ],
      "description": "ソート対象のカラム名"
    }
  }
}
```

#### リクエスト項目

| 項目 | タイプ | 説明 | パターン/列挙型 | 初期値 | 必須 |
| -------------- | :-------------: | --------------------------------------------------- | :--------------: | :---------: | :------: |
| `campaignId` | string | キャンペーンID | `^[0-9]+` | | * |
| `beginTimestamp` | string | 取得開始日時（UNIXタイムスタンプ、秒）。この時刻を含む | `^[0-9]+` | | * |
| `endTimestamp` | string | 取得終了日時（UNIXタイムスタンプ、秒）。この時刻を含まない | `^[0-9]+` | | * |
| `needMask` | string | フォーム入力値をマスクするか | `^[0-1]$` | `0` | |
| `page` | string / null | 取得するページ番号 | `^[0-9]+` | `1` | |
| `limit` | string / null | 1ページあたりの件数 | `^[0-9]+` | `100` | |
| `sort` | string / null | ソート順 | `desc`, `asc` | `desc` | |
| `sortBy` | string | ソート対象 | `createdAt`, `visitedAt` | `createdAt` | |

#### レスポンスの JSON-Schema

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "jsonSchema/behaviors/web/get.json",
  "type": "object",
  "title": "A schema for response body when request `GET /behaviors/web/webforms`.",
  "required": [
    "count",
    "total",
    "_embedded"
  ],
  "properties": {
    "count": {
      "title": "Number of webform behaviors in a response.",
      "type": "number"
    },
    "total": {
      "title": "Number of webform behaviors in a query.",
      "type": "number"
    },
    "_embedded": {
      "type": "object",
      "properties": {
        "webforms": {
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
| `count` | number | レスポンス内のフォームログ数 | `_embedded.webforms` の要素数 |
| `total` | number | 取得条件に一致するフォームログの総数 | |

##### フォームログ1件分の項目

`_embedded.webforms` は、JSONレスポンスに含まれるフォームログの配列です。以下はフォームログ1件分の既定の出力項目です。キャンペーンの設定によって変わります。

※ マスクが適用された場合、メールアドレスと電話番号はマスク処理後のフォーム入力値から抽出します。

| 項目 | タイプ | 説明 | 備考 |
|-----|-------|------|-----|
| `visitedAt` | date | ランディングページの閲覧日時 | |
| `media` | string | 広告種別 | ランディングページURLの `utm_medium`（未指定は `null`） |
| `source` | string | 広告媒体 | ランディングページURLの `utm_source`（未指定は `null`） |
| `keyword` | string | キーワード | ランディングページURLの `utm_term`（未指定は `null`） |
| `landingPageUrl` | string | ランディングページURL |  |
| `formUrl` | string | フォーム入力内容を記録したページのURL | |
| `defaultEmailAddress` | string | メールアドレス（※） | フォーム入力値のうち、メールアドレスの形式に一致する最初の値（該当なしは `null`） |
| `defaultHomePhoneNumber` | string | 固定電話番号（※） | フォーム入力値のうち、固定電話番号の形式に一致する最初の値（該当なしは `null`） |
| `defaultMobilePhoneNumber` | string | 携帯電話番号（※） | フォーム入力値のうち、携帯電話番号の形式に一致する最初の値（該当なしは `null`） |
| `timestamp` | number | フォーム入力内容の記録日時 | 計測タグは来訪者の端末時刻をUNIXタイムスタンプ（ミリ秒）で記録する。フォームまたは時刻の記録がなければ `null` |
| `forms` | array | フォーム入力内容 | フォームごとに1要素。入力項目の記録がないフォームの要素は `[]`、フォームの記録が0件なら `forms` 全体が `null` |

**`forms` の補足**

* 各要素は入力項目名をキー、フォーム入力値を値とします。値に含まれる半角スペースと改行はすべて削除されます。
* フォームを送信したとき、同じページにある別のフォームの入力内容も含まれる場合があります。
