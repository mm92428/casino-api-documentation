# /v4/user/create

## Create User

<mark style="color:green;">`POST`</mark> `/v4/user/create`

This endpoint creates and returns user information. The `name` must be unique; if a user with the same name already exists, it returns the existing user's information. The returned `user_code` value is used to identify the user in subsequent user-related API requests.

{% hint style="danger" %}
When converting from Transfer->Seamless or Seamless->Transfer, you cannot use the previously created user code. It is recommended to call it every time a user logs in to the site and receive and use the user code.
{% endhint %}

**Headers**

| Name          | Value                |
| ------------- | -------------------- |
| Authorization | `Bearer <API_TOKEN>` |
| Content-Type  | `application/json`   |
| Accept        | `application/json`   |

**Body**

```json
{
  "name": "test"
}
```

<table><thead><tr><th width="130">Name</th><th width="111">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>name</code></td><td>string</td><td>User's Name<br>Max length: 50<br>Min length: 2<br>Allowed characters: a~z, A~Z, 0~9, _</td></tr></tbody></table>

**Response**

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "user_code": 3
  }
}
```

| Key         | Type           | Description            |
| ----------- | -------------- | ---------------------- |
| `code`      | integer(32bit) | Response Code          |
| `message`   | string         | Response Message       |
| `user_code` | integer(64bit) | User's Code(Unique ID) |
