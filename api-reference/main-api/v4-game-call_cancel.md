# /v4/game/call\_cancel

## Cancel Bonus-Call

<mark style="color:green;">`POST`</mark> `/v4/game/call_start`

It cancels the bonus-call in progress. You must use the `call_id` returned when the bonus-call starts.

If it cancels a bonus-call for the other agent, the code `PERMISSION_ERROR` is returned. If the bonus-call has already ended, the code `BONUSCALL_ALEADY_ENDED` is returned.

**Headers**

| Name          | Value                |
| ------------- | -------------------- |
| Authorization | `Bearer <API_TOKEN>` |
| Content-Type  | `application/json`   |
| Accept        | `application/json`   |

**Body**

```json
{
  "call_id": 1
}
```

| Key       | Type           | Description          |
| --------- | -------------- | -------------------- |
| `call_id` | integer(64bit) | Unique Bonus-Call ID |

**Response**

```json
{
  "code": 0,
  "message": "OK"
}
```

| Key       | Type           | Description      |
| --------- | -------------- | ---------------- |
| `code`    | integer(32bit) | Response Code    |
| `message` | string         | Response Message |
