# /v4/game/round-details

## Get Round Detail URL

<mark style="color:green;">`POST`</mark> `/v4/game/round-details`

Returns the URL of the page that retrieves the round details of the transaction.

Round information up to one month (30 days) ago can be queried, and for rounds that cannot be found or detailed view is not supported, `ROUND_NOT_FOUND(2013)` is returned.

**Headers**

| Name          | Value                |
| ------------- | -------------------- |
| Authorization | `Bearer <API_TOKEN>` |
| Content-Type  | `application/json`   |
| Accept        | `application/json`   |

**Body**

```json
{
  "user_code": 3,
  "round_id": "12345713400010008",
  "provider_id": 1,
  "game_code": "vswaysdogs"
}
```

| Key           | Type           | Description |
| ------------- | -------------- | ----------- |
| `user_code`   | integer(64bit) | User's Code |
| `round_id`    | string         | Round ID    |
| `provider_id` | integer(64bit) | Provider ID |
| `game_code`   | string         | Game Symbol |

**Response**

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "url": "http://.../gs2c/parentRoundHistoryDetails.do?playSessionId=1252737599"
  }
}
```

| Key       | Type           | Description        |
| --------- | -------------- | ------------------ |
| `code`    | integer(32bit) | Response Code      |
| `message` | string         | Response Message   |
| `url`     | string         | Round Detailed URL |
