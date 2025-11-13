# Main API

## Required HTTP Headers

All HTTP requests related to API must include the following HTTP headers.

<table><thead><tr><th width="167">Name</th><th width="186">Value</th><th>Example</th></tr></thead><tbody><tr><td>Authorization</td><td>Bearer {API_TOKEN} </td><td><mark style="color:green;"><code>Authorization: Bearer ed45553e-1702-4342-a70d-09bf7ff7f568</code></mark></td></tr><tr><td>Accept</td><td>application/json</td><td><mark style="color:orange;"><code>Accept: application/json</code></mark></td></tr><tr><td>Content-Type</td><td>application/json</td><td><mark style="color:red;"><code>Content-Type: application/json</code></mark></td></tr></tbody></table>

## Language Codes

The SlotCity API currently supports the following languages.

| Value | Code | Language   |
| ----- | ---- | ---------- |
| 1     | en   | English    |
| 2     | ko   | Korean     |
| 3     | th   | Thai       |
| 4     | es   | Spanish    |
| 5     | ja   | Japanese   |
| 6     | pt   | Portuguese |
| 7     | tr   | Turkish    |
| 8     | de   | German     |
| 9     | id   | Indonesian |
| 10    | ms   | Malay      |
| 11    | zh   | Chinese    |

## Transaction Types

All bets and wins in the game are recorded as transactions.

Even if there is no win, a Win(2) transaction with a value of 0 will still occur.

If an error occurs while processing a Bet(1) transaction using the callback URL, a BetCancel(16) transaction will be triggered, and the bet amount will be refunded.

Below are the transaction types returned by the API.

| Value | Transaction | Description        |
| ----- | ----------- | ------------------ |
| 1     | Bet         | Betting            |
| 16    | BetCancel   | Cancel Betting     |
| 2     | Win         | Winning            |
| 32    | BonusCall   | Bonus-Call Winning |
| 4     | Deposit     | Point Deposit      |
| 8     | Withdraw    | Point Withdraw     |

{% hint style="info" %}
To manage server load, transactions made two days before the maintenance period will be automatically deleted.
{% endhint %}

## Provider Maintenance

When the game provider enters maintenance mode, the status Maintenance(2) is returned.

If you try to access the game during maintenance, the response code UNDER\_MAINTENANCE(1) is returned.

Below are the provider and game statuses returned by the API.

| Value | Status      | Description    |
| ----- | ----------- | -------------- |
| 1     | Normal      | On normal      |
| 2     | Maintenance | On maintenance |

## API Response Codes

Below are the response codes returned when calling the API.

<table><thead><tr><th width="265">Code</th><th width="94">Value</th><th>Description</th></tr></thead><tbody><tr><td>OK</td><td>0</td><td>Success</td></tr><tr><td>UNDER_MAINTENANCE</td><td>1</td><td>On maintenance</td></tr><tr><td>INTERNAL_SERVER_ERROR</td><td>1001</td><td>Internal server error</td></tr><tr><td>VALIDATION_ERROR</td><td>1002</td><td>Request data format error</td></tr><tr><td>SERVICE_EXCEPTION</td><td>1003</td><td>Service exception occurred</td></tr><tr><td>TOKEN_NOT_FOUND</td><td>1007</td><td>Authentication token not found</td></tr><tr><td>TOKEN_INVALID</td><td>1009</td><td>Authentication token invalid</td></tr><tr><td>PERMISSION_ERROR</td><td>1010</td><td>No access</td></tr><tr><td>PROVIDER_ERROR</td><td>1011</td><td>Provider error</td></tr><tr><td>PARAMETERS_INVALID</td><td>1012</td><td>Parameters invalid</td></tr><tr><td>CALLBACK_ERROR</td><td>1015</td><td>Callback error (<em>Callback API error log needs to be checked</em>)</td></tr><tr><td>SERVER_IS_BUSY</td><td>1018</td><td>Server load error (<em>concurrent call limit exceeded</em>)</td></tr><tr><td>IP_NOT_ALLOWED</td><td>1020</td><td>Not allowed IP</td></tr><tr><td>AGENT_NOT_FOUND</td><td>2001</td><td>Agent information not found</td></tr><tr><td>USER_NOT_FOUND</td><td>2002</td><td>User information not found</td></tr><tr><td>GAME_NOT_FOUND</td><td>2003</td><td>Game information not found</td></tr><tr><td>POINT_NOT_ENOUGH</td><td>2005</td><td>Insufficient agent point balance</td></tr><tr><td>BALANCE_NOT_ENOUGH</td><td>2006</td><td>Insufficient user point balance</td></tr><tr><td>PROVIDER_NOT_FOUND</td><td>2007</td><td>Provider information not found</td></tr><tr><td>BONUSCALL_DOUBLE</td><td>2011</td><td>Already bonus call is running</td></tr><tr><td>BONUSCALL_ALEADY_ENDED</td><td>2012</td><td>Already bonus call is ended</td></tr><tr><td>ROUND_NOT_FOUND</td><td>2013</td><td>Round information not found</td></tr><tr><td>CURRENCY_NOT_SUPPORTED</td><td>2014</td><td>Currency is not supported</td></tr></tbody></table>
