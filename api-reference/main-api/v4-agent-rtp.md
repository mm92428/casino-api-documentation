# /v4/agent/rtp

## Change Agent's RTP

<mark style="color:green;">`POST`</mark> `/v4/agent/rtp`

It changes Agent's RTP. RTP is applied to all users of the agent, with the exception of requests for individual RTP when running the game.

**Headers**

| Name          | Value                |
| ------------- | -------------------- |
| Authorization | `Bearer <API_TOKEN>` |
| Content-Type  | `application/json`   |
| Accept        | `application/json`   |

**Body**

```json
{
  "win_ratio": 85
}
```

| Name        | Type  | Description          |
| ----------- | ----- | -------------------- |
| `win_ratio` | float | RTP value (75 \~ 95) |

**Response**

```json
{
  "code": 0,
  "message": "OK"
}
```

| Key     | Type           | Description      |
| ------- | -------------- | ---------------- |
| code    | integer(32bit) | Response Code    |
| message | string         | Response Message |
