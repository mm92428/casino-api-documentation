# /v4/game/kick\_games

## Kick User's Games

<mark style="color:green;">`POST`</mark> `/v4/game/kick_games`

Terminates all active gaming sessions for the user. The session will be terminated when a new round of betting is placed.

**Headers**

| Name          | Value                |
| ------------- | -------------------- |
| Authorization | `Bearer <API_TOKEN>` |
| Content-Type  | `application/json`   |
| Accept        | `application/json`   |

**Body**

```json
{
  "user_code": 3
}
```

| Key        | Type           | Description |
| ---------- | -------------- | ----------- |
| user\_code | Integer(64bit) | User's Code |

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
