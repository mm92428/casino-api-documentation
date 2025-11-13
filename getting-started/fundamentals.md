# Fundamentals

Once you have taken a glance at our API, let's move forward to the next step right now.

## What You Need To Start?

We assume you already have knowledge of RESTful APIs and programming languages such as `Node.js` or `PHP`.

You will need an `API_TOKEN` issued by our administrators, which serves as your identity to bypass our security checks. Please keep your token string confidential to prevent any potential malicious activities. We are not responsible if you expose your token string.

{% hint style="info" %}
API\_TOKEN string looks like this: <mark style="color:red;">`ed45553e-1702-4342-a70d-09bf7ff7f568`</mark>
{% endhint %}

We will provide you with one or more accounts for back office login. You can find your `API_TOKEN` string in the back office. Accounts will be issued based on your currency requirements. If you need support for multiple currencies, you will require separate accounts for each currency, as each account is designated for a single currency.

Please [contact us](https://scorpioplay.com/contact) to obtain your accounts.

{% hint style="info" %}
Our back office URL is <https://sc4-admin.dreamgates.net/>
{% endhint %}

## HTTP Headers

All HTTP requests related to API must include the following HTTP headers.

You can check the API\_TOKEN value in the provided manager account's settings and information.

<table><thead><tr><th width="167">Name</th><th width="186">Value</th><th>Example</th></tr></thead><tbody><tr><td>Authorization</td><td>Bearer {API_TOKEN} </td><td><mark style="color:green;"><code>Authorization: Bearer ed45553e-1702-4342-a70d-09bf7ff7f568</code></mark></td></tr><tr><td>Accept</td><td>application/json</td><td><mark style="color:orange;"><code>Accept: application/json</code></mark></td></tr><tr><td>Content-Type</td><td>application/json</td><td><mark style="color:red;"><code>Content-Type: application/json</code></mark></td></tr></tbody></table>

## System Security

In preparation for an attack on this system, simultaneous calls from the same agent are limited to 50 times, and if exceeded, `SERVER_IS_BUSY(1018)` is returned.&#x20;

When this code returns, you must wait for the existing call to complete.

## IP Restriction

The API can only be called from servers with IP addresses specified in the <mark style="color:blue;">\[Settings]</mark> page of our back office. Enter each IP address separated by a space or a new line (e.g., 1.1.1.1 2.2.2.2). If you do not wish to restrict access by IP, leave the \[Allowed IP] field blank.

## Basic Workflow

### Creating Users

You need to create users in our API first, as we need to identify which user is playing our game. While you may already have a user table in your casino database, our API will synchronize with your existing user data to ensure accurate identification.

Once a user is created, we assign a unique integer called `user_code`. This can be used as the player's unique ID.

### Game Launch

Everything begins when a player opens one of our games. At that point, you will need to call our API to retrieve a one-time-use game URL. Once the URL is generated, your casino will redirect the player to our game using this URL. From there, the player can place bets and experience wins or losses.

### Bet and Win/Lose

Once the game is launched, the player will start playing. When they place a bet, our API will generate the result based on your casino's RTP settings and record the transaction history. At this stage, our API will need to update the player's balance. If a bet or win/loss is not processed correctly due to unexpected issues, all funds will be refunded to prevent any loss for your players.

## Agent Subsystems

The term <mark style="color:red;">**'Agent'**</mark> refers to a single casino website operator. If you manage multiple casinos, you are referred to as an <mark style="color:red;">**'Operator'**</mark>.&#x20;

An Operator can have multiple Agents under their control, forming a hierarchy. Each Agent operates independently and has their own account for our back office.

As an Operator, you can create as many sub-agents as needed and manage all Agents under your supervision. If you are a broker looking to resell our API, you can create multiple Operators under your management.

{% hint style="info" %}
You can mange your sub-agents on <mark style="color:blue;">\[Agent/Agents Treeview]</mark> page of our back office.
{% endhint %}

## Balance System

Each agent in the system is assigned a digital wallet. This wallet functions as a repository for their credits, which are referred to as <mark style="color:red;">**'points'**</mark>. These points are essential for the operation of the agent's casino activities. Without sufficient points in their wallet, an agent cannot run or manage their casino operations effectively.

Points can be acquired from higher-level operators, often referred to as parent operators. These parent operators distribute points to their agents, enabling them to maintain and operate their casinos. Essentially, the flow of points from parent operators to agents is crucial for the continuous functioning of the casino system.

***

The SlotCity API offers 2 integration models: **Seamless Mode** and **Money Transfer Mode**

<table data-card-size="large" data-view="cards" data-full-width="false"><thead><tr><th data-type="content-ref"></th><th data-hidden align="center"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td><a href="seamless-mode">seamless-mode</a></td><td align="center"></td><td><a href="https://3590886847-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FsQaLK3pikuFct6LKHcJX%2Fuploads%2FgZpFH4JZFm3PfXQldno8%2Fseamless.jpeg?alt=media&#x26;token=e016c2fd-bde0-4811-a846-e192cecdaec7">seamless.jpeg</a></td></tr><tr><td><a href="money-transfer-mode">money-transfer-mode</a></td><td align="center"></td><td><a href="https://3590886847-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FsQaLK3pikuFct6LKHcJX%2Fuploads%2F7wIpeDTo5Ub27Z68zxuD%2Fmoney_transfer_mode.jpeg?alt=media&#x26;token=3e38a0d8-e2e2-4b71-83bb-045080aa5a64">money_transfer_mode.jpeg</a></td></tr></tbody></table>
