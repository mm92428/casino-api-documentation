# /v4/game/online-games

## Get Online-Game List

<mark style="color:green;">`POST`</mark> `/v4/game/online-games`

It returns a list of games connected up to 30 minutes ago from the current time.

The returned game-play unique ID `gplay_id` can be used to start a bonus call or manage the game in progress.

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
  "data": [
    {
      "gplay_id": 1,
      "user_code": 400000001,
      "user_name": "test1",
      "provider_id": 1,
      "provider_name": "Pragmatic Play",
      "game_code": "vswaysdogs",
      "game_name": "The Dog House Megaways",
      "category": 1,
      "last_round_bet": 0,
      "spend": 10000,
      "win": 1000,
      "call_win": 0,
      "call_id": 0,
      "call_enable": true,
      "callend_flag": true,
      "call_status": 0,
      "start_time": "2023.01.13 12:01:01",
      "last_update": "2023.01.13 12:01:01"
    }
  ]
}
```

| Key              | Type             | Description                                                             |
| ---------------- | ---------------- | ----------------------------------------------------------------------- |
| `code`           | integer(32bit)   | Response Code                                                           |
| `message`        | string           | Response Message                                                        |
| `gplay_id`       | string           | Game Launch URL                                                         |
| `user_code`      | integer(64bit)   | User's Code(Unique ID)                                                  |
| `user_name`      | string           | User's Name                                                             |
| `provider_id`    | integer(64bit)   | Provider ID                                                             |
| `provider_name`  | string           | Provider Name (English)                                                 |
| `game_code`      | string           | Game Symbol                                                             |
| `game_name`      | string           | Game Name (English)                                                     |
| `category`       | integer(32bit)   | Game Type (1: Slot, 2: Live)                                            |
| `last_round_bet` | double           | Last Round Bet Amount                                                   |
| `spend`          | double           | Total Bet Amount                                                        |
| `win`            | double           | Total Win (Excluding Bonus-Call Win)                                    |
| `call_win`       | double           | Bonus-Call Win                                                          |
| `call_id`        | integer(64bit)   | Bonus-Call ID In Progress                                               |
| `call_enable`    | boolean          | Bonus-Call Enabled                                                      |
| `callend_flag`   | boolean          | Bonus-Call Finished                                                     |
| `call_status`    | integer(32bit)   | <p>Bonus-Call Status<br>0: Init, 1: Running, 2: Complete, 3: Cancel</p> |
| `start_time`     | string(ISO date) | Start Time                                                              |
| `last_update`    | string(ISO date) | Last Updated Time                                                       |
