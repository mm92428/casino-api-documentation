# /v4/game/game-url

## Get Game Access URL

<mark style="color:green;">`POST`</mark> `/v4/game/game-url`

It returns the URL to access the game. The URL is valid for 10 minutes from creation and is for one-time use only.

When making a request, the `game_symbol` value can be used as the `game_code` from the game list.

To apply a user-specific RTP instead of the agent's RTP, specify the desired RTP in `win_ratio`. This value must be equal to or less than the agent's RTP. To use the agent's RTP, set `win_ratio` to 0 (default).

If you use a `user_code` from a user belonging to another agent, a `PERMISSION_ERROR` code is returned. If you attempt to run a game not assigned to the agent, a `PROVIDER_NOT_FOUND` or `GAME_NOT_FOUND` code is returned.

**Headers**

| Name          | Value                |
| ------------- | -------------------- |
| Authorization | `Bearer <API_TOKEN>` |
| Content-Type  | `application/json`   |
| Accept        | `application/json`   |

**Body**

```json
{
  "user_code": 400000001,
  "provider_id": 1,
  "game_symbol": "vswaysdogs",
  "lang": 1,
  "return_url": "https://lobby.slotcity.com",
  "win_ratio": 0
}
```

<table><thead><tr><th width="171">Name</th><th width="151">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>user_code</code></td><td>integer(64bit)</td><td>User's Code</td></tr><tr><td><code>provider_id</code></td><td>integer(64bit)</td><td>Provider ID</td></tr><tr><td><code>game_symbol</code></td><td>string</td><td>Game Symbol</td></tr><tr><td><code>lang</code></td><td>integer(32bit)</td><td>Language Code Value<br>For more information, please refer to <a href="..#language-codes">this</a> page.</td></tr><tr><td><code>return_url</code></td><td>string</td><td>Home Url (Redirect URL when the game is closed)</td></tr><tr><td><code>win_ratio</code></td><td>float</td><td>RTP(Winning Rate)</td></tr></tbody></table>

**Response**

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "game_url": "http://..."
  }
}
```

| Key        | Type           | Description      |
| ---------- | -------------- | ---------------- |
| `code`     | integer(32bit) | Response Code    |
| `message`  | string         | Response Message |
| `game_url` | string         | Game Launch URL  |
