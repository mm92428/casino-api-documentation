# /v4/game/providers

## Get Providers List

<mark style="color:green;">`POST`</mark> `/v4/game/providers`

It returns the list of providers assigned to the agent. If the empty list is returned, you can inquire about connecting the providers from the upper agent.

**Headers**

| Name          | Value                |
| ------------- | -------------------- |
| Authorization | `Bearer <API_TOKEN>` |
| Content-Type  | `application/json`   |
| Accept        | `application/json`   |

**Body**

```json
{
  "lang": 1
}
```

<table><thead><tr><th width="143">Name</th><th width="143">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>lang</code></td><td>integer(32bit)</td><td>Language Code Value<br>For more information, please refer to <a href="..#language-codes">this</a> page.</td></tr></tbody></table>

**Response**

```json
{
  "code": 0,
  "message": "OK",
  "data": [
    {
      "provider_id": 1,
      "provider_name": "Pragmatic Play",
      "locale_name": "Pragmatic Play",
      "status": 1
    }
  ]
}
```

| Key             | Type           | Description                                             |
| --------------- | -------------- | ------------------------------------------------------- |
| `code`          | integer(32bit) | Response Code                                           |
| `message`       | string         | Response Message                                        |
| `provider_id`   | integer(64bit) | Provider ID                                             |
| `provider_name` | string         | Provider Name (English)                                 |
| `locale_name`   | string         | Provider Name (Localized)                               |
| `status`        | integer(32bit) | <p>Provider's Status<br>1: Normal<br>2: Maintenance</p> |
