# /v4/game/transaction

## Search Game Transactions (Search By Time)

<mark style="color:green;">`POST`</mark> `/v4/game/transaction`

It searchs the game transaction list with a search time of up to 1 hour (Korea time-Seoul). The request list can be set up to 2000.

\[1] Let `start_time` and `end_time` be the times you want to search. Example:`2023-03-01 11:00:00` \~ `2023-03-01 12:00:00`

\[2] The `start_time` of the next request is set to the previously requested `end_time` and the request is made. Example:`2023-03-01 12:00:00` \~ `2023-03-01 13:00:00`

\[3] The next request must be delayed by more than `30 seconds`.

**Headers**

| Name          | Value                |
| ------------- | -------------------- |
| Authorization | `Bearer <API_TOKEN>` |
| Content-Type  | `application/json`   |
| Accept        | `application/json`   |

**Body**

```json
{
  "start_time": "2022-12-26 15:00:00",
  "end_time": "2022-12-26 16:00:00",
  "offset": 0,
  "limit": 10
}
```

| Key          | Type           | Description                                    |
| ------------ | -------------- | ---------------------------------------------- |
| `start_time` | string         | Start Time For Searching (yyyy-MM-dd HH:mm:ss) |
| `end_time`   | string         | End Time For Searching (yyyy-MM-dd HH:mm:ss)   |
| `offset`     | integer(32bit) | Start Position In List                         |
| `limit`      | integer(32bit) | Length of List (MAX 2000)                      |

**Response**

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "total": 100,
    "offset": 0,
    "count": 100,
    "list": [
      {
        "trans_id": 101,
        "user_code": 400000001,
        "round_id": "12345713400010008",
        "trans_type": 1,
        "provider_id": 1,
        "provider_name": "Pragmatic Play",
        "game_code": "vswaysdogs",
        "game_name": "The Dog House Megaways",
        "category": "Slots",
        "prebalance": 45000,
        "trans_amount": 45000,
        "balance": 50000,
        "regdate": "2022-12-28 12:00:00"
      }
    ]
  }
}
```

| Key             | Type           | Description                                                                        |
| --------------- | -------------- | ---------------------------------------------------------------------------------- |
| `code`          | integer(32bit) | Response Code                                                                      |
| `message`       | string         | Response Message                                                                   |
| `total`         | integer(32bit) | Total Count                                                                        |
| `offset`        | integer(32bit) | Start Position In List                                                             |
| `count`         | integer(32bit) | List Count                                                                         |
| `list`          | array          | List Data                                                                          |
| `trans_id`      | integer(64bit) | Transaction ID                                                                     |
| `user_code`     | integer(64bit) | User's Code                                                                        |
| `round_id`      | string         | Round ID                                                                           |
| `trans_type`    | integer(32bit) | <p>User's Transaction<br>See All Types <a href="..#transaction-types">Here</a></p> |
| `provider_id`   | integer(64bit) | Provider ID                                                                        |
| `provider_name` | string         | Provider Name (English)                                                            |
| `game_code`     | string         | Game Symbol                                                                        |
| `game_name`     | string         | Game Name (English)                                                                |
| `category`      | string         | Game Category                                                                      |
| `prebalance`    | double         | Balance before transaction                                                         |
| `trans_amount`  | double         | Transaction Amount                                                                 |
| `balance`       | double         | Balance after transaction                                                          |
| `regdate`       | string         | Transaction registration time                                                      |
