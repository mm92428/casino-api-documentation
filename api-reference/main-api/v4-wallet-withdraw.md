# /v4/wallet/withdraw

## Withdraw User Balance

<mark style="color:green;">`POST`</mark> `/v4/wallet/withdraw`

It withdraws the user balance. The money withdrew from the user is added to the agent's points.

**Headers**

| Name          | Value                |
| ------------- | -------------------- |
| Authorization | `Bearer <API_TOKEN>` |
| Content-Type  | `application/json`   |
| Accept        | `application/json`   |

**Body**

```json
{
  "user_code": 3,
  "amount": 50000
}
```

<table><thead><tr><th width="143">Name</th><th width="143">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>user_code</code></td><td>integer(64bit)</td><td>User's code(unique ID)</td></tr><tr><td><code>amount</code></td><td>double</td><td>Withdraw amount</td></tr></tbody></table>

**Response**

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "balance": 26500,
    "amount": 0
  }
}
```

| Key       | Type           | Description            |
| --------- | -------------- | ---------------------- |
| `code`    | integer(32bit) | Response Code          |
| `message` | string         | Response Message       |
| `balance` | double         | Point After Processing |
| `amount`  | double         | Processing Amount      |
