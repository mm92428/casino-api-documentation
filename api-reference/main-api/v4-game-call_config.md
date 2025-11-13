# /v4/game/call\_config

## Check Bonus-Call Settings

<mark style="color:green;">`POST`</mark> `/v4/game/call_config`

Check the settings for applying the bonus-call.

When starting the bonus-call, the bonus-call must be applied with an amount equal to or greater than the minimum call amount `call_min` returned.

**Headers**

| Name          | Value                |
| ------------- | -------------------- |
| Authorization | `Bearer <API_TOKEN>` |
| Content-Type  | `application/json`   |
| Accept        | `application/json`   |

**Body**

No Contents

**Response**

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "call_min": 5000
  }
}
```

| Key        | Type           | Description               |
| ---------- | -------------- | ------------------------- |
| `code`     | integer(32bit) | Response Code             |
| `message`  | string         | Response Message          |
| `call_min` | double         | Bonus-Call Minimum Amount |
