# /v4/user/info

## Get User Information

<mark style="color:green;">`POST`</mark> `/v4/user/info`

It returns the user information (name, balance amount, etc.).\
If there is no user information, the code `USER_NOT_FOUND` is returned. If you try to search the user information of another agent, the code `PERMISSION_ERROR` is returned.

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

<table><thead><tr><th width="143">Name</th><th width="143">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>user_code</code></td><td>integer(64bit)</td><td>User's Code(Unique ID)</td></tr></tbody></table>

**Response**

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "name": "test",
    "balance": 26500
  }
}
```

| Key       | Type           | Description      |
| --------- | -------------- | ---------------- |
| `code`    | integer(32bit) | Response Code    |
| `message` | string         | Response Message |
| `name`    | string         | User's Name      |
| `balance` | double         | User's Point     |
