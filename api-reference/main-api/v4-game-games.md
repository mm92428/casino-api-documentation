# /v4/game/games

## Get Games List

<mark style="color:green;">`POST`</mark> `/v4/game/games`

It returns the list of games assigned to the agent. If the list is empty, you can inquire about connecting the games from the parent agent. It requests the games list using the `provider_id` from the providers list.

The list includes both slot and live games. If it is a slot game, the category is set to "Slots"; if it is a live game, the category is set to "Live".

**Headers**

| Name          | Value                |
| ------------- | -------------------- |
| Authorization | `Bearer <API_TOKEN>` |
| Content-Type  | `application/json`   |
| Accept        | `application/json`   |

**Body**

```json
{
  "provider_id": 1,
  "lang": 1
}
```

<table><thead><tr><th width="171">Name</th><th width="151">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>provider_id</code></td><td>integer(32bit)</td><td>Provider ID</td></tr><tr><td><code>lang</code></td><td>integer(32bit)</td><td>Language Code Value<br>For more information, please refer to <a href="..#language-codes">this</a> page.</td></tr></tbody></table>

**Response**

```json
{
  "code": 0,
  "message": "OK",
  "data": [
    {
      "provider_id": 1,
      "game_code": "vswaysdogs",
      "game_name": "The Dog House Megaways",
      "locale_name": "The Dog House Megaways",
      "game_image": "http://.../game_pic/en/216x160/vswaysdogs.jpg",
      "launch_enable": true,
      "category": "Slots",
      "reg_date": "2024-09-19T19:47:20.960Z"
    }
  ]
}
```

| Key             | Type           | Description                                                |
| --------------- | -------------- | ---------------------------------------------------------- |
| `code`          | integer(32bit) | Response Code                                              |
| `message`       | string         | Response Message                                           |
| `provider_id`   | integer(64bit) | Provider ID                                                |
| `game_code`     | string         | Game Symbol                                                |
| `game_name`     | string         | Game Name (English)                                        |
| `locale_name`   | string         | Game Name (Localized)                                      |
| `game_image`    | string         | Game Image URL                                             |
| `launch_enable` | boolean        | Game Launchable                                            |
| `category`      | string         | Game Category, ‘Slots’ for slot game, ‘Live’ for live game |
| `reg_date`      | string         | Game Registration Date                                     |
