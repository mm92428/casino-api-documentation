# /v4/agent/info

## Get Agent Information

<mark style="color:green;">`POST`</mark> `/v4/agent/info`

It returns agent information (name, balance amount, etc.).

**Headers**

| Name          | Value                |
| ------------- | -------------------- |
| Authorization | `Bearer <API_TOKEN>` |
| Content-Type  | `application/json`   |
| Accept        | `application/json`   |

**Body**

No contents

**Response**

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "name": "dreamgames",
    "currency": 2,
    "balance": 50000,
    "win_ratio": 95.5
  }
}
```

| Key        | Data Type      | Description               |
| ---------- | -------------- | ------------------------- |
| code       | integer(32bit) | Response Code             |
| message    | string         | Response Message          |
| name       | string         | Agent's Name              |
| currency   | integer(32bit) | Agent's Currency          |
| balance    | double         | Agent's Point             |
| win\_ratio | float          | Agent's RTP(Winning Rate) |
