# /v4/game/transaction-id

## Search Game Transactions (Search By Transaction ID)

<mark style="color:green;">`POST`</mark> `/v4/game/transaction-id`

The search is performed from the corresponding transaction ID.

\[1] When calling by the same `last_id`, a delay of `30 seconds` or more is required.

\[2] The `last_id` value is initially requested as `0` and then requested again with the returned ID value.

\[3] The resulting data excludes the transaction with the `last_id` value, and returns the transaction created after that.

\[4] The latest data is located at the end.

**Headers**

| Name          | Value                |
| ------------- | -------------------- |
| Authorization | `Bearer <API_TOKEN>` |
| Content-Type  | `application/json`   |
| Accept        | `application/json`   |

**Body**

```json
{
  "last_id": 0,
  "limit": 10
}
```

| Key       | Type           | Description               |
| --------- | -------------- | ------------------------- |
| `last_id` | integer(64bit) | Starting Transaction ID   |
| `limit`   | integer(32bit) | Length of List (MAX 2000) |

**Response**

```json
{
  "code": 0,
  "message": "OK",
  "data": [
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
```

| Key             | Type           | Description                                                                        |
| --------------- | -------------- | ---------------------------------------------------------------------------------- |
| `code`          | integer(32bit) | Response Code                                                                      |
| `message`       | string         | Response Message                                                                   |
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
