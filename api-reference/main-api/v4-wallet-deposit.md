# /v4/wallet/deposit

## Deposit User Balance

<mark style="color:green;">`POST`</mark> `/v4/wallet/deposit`

It deposits the user balance. The money deposited to the user is deducted from the agent's points.

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

<table><thead><tr><th width="143">Name</th><th width="143">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>user_code</code></td><td>integer(64bit)</td><td>User's code(unique ID)</td></tr><tr><td><code>amount</code></td><td>double</td><td>Deposit amount</td></tr></tbody></table>

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
