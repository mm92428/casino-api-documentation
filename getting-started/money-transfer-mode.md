# Money Transfer Mode

## About Money Transfer Mode

Money Transfer Mode is an integration method where all player funds are transferred to our API, and we manage their balances. When players deposit or withdraw, their funds are stored in our database. You simply need to call our API to retrieve or update player balances.

## What Is Different From Seamless Integration?

Money Transfer Mode:&#x20;

• Funds Management: All player funds are transferred to our API, and we manage their balances.

• Deposits/Withdrawals: Player funds are stored in our database.

• API Calls: Simply call our API, receive the result, and you're done! No additional steps required.

Seamless Integration:

&#x20;• Funds Management: Player funds remain in your system, and we interact with your backend to manage transactions.

• Deposits/Withdrawals: Player funds are stored in your database.

• API Calls: When you call our Main API, we then contact your backend to process transactions and update balances. Therefore, an additional step is involved.

{% hint style="info" %}
To enable this mode, ensure the Callback URL field is left empty on the <mark style="color:blue;">\[Settings]</mark> page in our back office.
{% endhint %}

To integrate our API with Money Transfer Mode, please refer to the [Main API](https://slotcity.gitbook.io/slotcity-api-documentation/api-reference/openapi) page.
