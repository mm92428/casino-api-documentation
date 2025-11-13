# /v4/game/call\_start

## Start Bonus-Call

<mark style="color:green;">`POST`</mark> `/v4/game/call_start`

It applies the bonus-call to the game currently being played. You must use the `gplay_id` from the online-game list.

If the call function is disabled for an agent, the `PERMISSION_ERROR` code is returned.

**Headers**

| Name          | Value                |
| ------------- | -------------------- |
| Authorization | `Bearer <API_TOKEN>` |
| Content-Type  | `application/json`   |
| Accept        | `application/json`   |

**Body**

```json
{
  "gplay_id": 1,
  "set_point": 10000,
  "memo": "string"
}
```

| Key         | Type           | Description              |
| ----------- | -------------- | ------------------------ |
| `gplay_id`  | integer(64bit) | Game-Play ID In Progress |
| `set_point` | double         | Bonus-Call Amount        |
| `memo`      | string         | Bonus-Call Memo          |

**Response**

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "call_id": 1
  }
}
```

| Key       | Type           | Description           |
| --------- | -------------- | --------------------- |
| `code`    | integer(32bit) | Response Code         |
| `message` | string         | Response Message      |
| `call_id` | integer(64bit) | Started Bonus-Call ID |
