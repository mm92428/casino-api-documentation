# Callback Responses

{% hint style="info" %}
If you use [Money Transfer Mode](https://slotcity.gitbook.io/slotcity-api-documentation/getting-started/money-transfer-mode), you don't need to read this page.
{% endhint %}

Once our API sends a callback request to your casino's backend, you need to parse it accurately and respond appropriately.

Before parsing the JSON data from our callback request body, you need to validate certain data in your database. To do this, first read the `check` field, split the string by commas, and validate each check case accordingly.

{% hint style="danger" %}
The JSON structure of your response is critical for our API, as we rely on predefined rules to parse it correctly. Please ensure that your response adheres to the specified JSON format. Many developers overlook this detail and subsequently encounter 'CALLBACK\_ERROR'.
{% endhint %}

{% hint style="danger" %}
**Timeout Caution**

* In case of COMMAND `bet` and `balance`, the response must be delivered <mark style="color:red;">within 2 seconds</mark>
* For the other COMMANDs, the response must be delivered <mark style="color:red;">within 4 seconds</mark>
  {% endhint %}

## Validation Response Formats

There should be 5 types of validation in your code.

<table data-full-width="false"><thead><tr><th width="97">Check</th><th width="221">Description</th><th>Response Format (If Validation Fails)</th></tr></thead><tbody><tr><td>21</td><td>Check if a player is authenticated.</td><td><code>{"result": 21, "status": "ERROR"}</code></td></tr><tr><td>22</td><td>Check if a player is activated.</td><td><code>{"result": 22, "status": "ERROR"}</code></td></tr><tr><td>31</td><td>Check if a player's balance is sufficient.</td><td><code>{"result": 31, "status": "ERROR", "data": {"balance": 0}}</code></td></tr><tr><td>41</td><td>Check if a transaction has already been processed.</td><td><code>{"result": 41, "status": "ERROR", "data": {"balance": 0}}</code></td></tr><tr><td>42</td><td>Check if a transaction is initialized.</td><td><code>{"result": 42, "status": "ERROR", "data": {"balance": 0}}</code></td></tr></tbody></table>

## Command Response Formats

If validation is successful, you can execute actions based on the `command` codes provided in our callback request body.

{% hint style="info" %}
The `authenticate`, `balance`, `status` commands will always succeed, if validation is passed.
{% endhint %}

<table><thead><tr><th width="142">Command</th><th>Response Format (Success)</th><th>Response Format (Fail)</th></tr></thead><tbody><tr><td>authenticate</td><td><code>{"result": 0, "status": "OK", "data": {"account": "test1234", "balance": 100}}</code></td><td></td></tr><tr><td>balance</td><td><code>{"result": 0, "status": "OK", "data": {"balance": 100}}</code></td><td></td></tr><tr><td>bet</td><td><code>{"result": 0, "status": "OK", "data": {"balance": 100}}</code></td><td><code>{"result": 99, "status": "ERROR", "data": {"balance": 100}}</code></td></tr><tr><td>win</td><td><code>{"result": 0, "status": "OK", "data": {"balance": 100}}</code></td><td><code>{"result": 99, "status": "ERROR", "data": {"balance": 100}}</code></td></tr><tr><td>cancel</td><td><code>{"result": 0, "status": "OK", "data": {"balance": 100}}</code></td><td><code>{"result": 99, "status": "ERROR", "data": {"balance": 100}}</code></td></tr><tr><td>status</td><td><p><strong>Transaction is succeeded:</strong></p><p><code>{"result": 0, "status": "OK", "data": {"trans_id": 1676358191, "trans_status: "OK"}}</code><br><strong>Transaction is canceled:</strong><br><code>{"result": 0, "status": "OK", "data": {"trans_id": 1676358191, "trans_status: "CANCELED"}}</code></p></td><td></td></tr></tbody></table>

For a deeper understanding of how to parse our callback request, please refer to the [Seamless Integration Code Examples](https://slotcity.gitbook.io/slotcity-api-documentation/api-reference/integrations) page.
