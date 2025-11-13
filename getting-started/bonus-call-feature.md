# Bonus Call Feature

{% hint style="info" %}
Please review this page to understand the Bonus Call feature, an essential tool for attracting new players to your newly opened casino.
{% endhint %}

## What Is A Bonus Call?

The Bonus Call feature is crafted to elevate player engagement by allowing temporary bonuses to be added to ongoing games, excluding live games.&#x20;

This functionality enables agents to offer appealing bonuses to players, thereby enhancing their gaming experience and encouraging continued play.&#x20;

Crucially, these bonuses are applied in a manner that does not affect the agents' overall points balance, ensuring they can maintain their resources while still providing attractive incentives.

## How It Works?

### Process

The Bonus Call feature can be activated for a specific user within a particular game.&#x20;

To begin, identify online users currently engaged in games. You can find these users in the <mark style="color:blue;">\[Users/Game Connections]</mark> page of our back office, or by calling the endpoint - [v4/game/online-games](https://slotcity.gitbook.io/slotcity-api-documentation/api-reference/openapi/v4-game-call_start).

Next, specify the bonus amount you wish to allocate to the players. You also have the option to include a memo for additional information.

Once the Bonus Call is initiated, the player will receive the bonus amount over several spins or free spins. Regardless of the amount of spins, the bonus amount will be credited to the player's account.

When a bonus call is activated, the bonus amount is temporarily deducted from the agent's points and credited to the user's balance. If the user wins, the winning amount is paid out to the user, and the bonus amount is eventually returned to the agent's points. This ensures that the agent's points remain unaffected by the bonus call process.

### Eligibility

Bonus calls can be applied to games where the `call_enable` flag is set to true in the in-progress game list.

### Impact On RTP

The betting amount during a bonus call is included in the RTP (Return to Player) calculations, ensuring that the overall RTP is not impacted by the bonus call transactions.

### Transaction Types

If a user wins through a bonus call, a BonusCall(32) transaction is generated instead of the usual Win(2) transaction.

### Seamless Mode

In seamless mode, a bonus call ID is sent to the betting/winning callback. This ID value is returned as `call_id`, with a value of 0 indicating a general transaction.

### Status Monitoring

The current status of a bonus call can be checked using the `call_status` field in the in-progress game list. This field provides detailed information about the status of the bonus call.

The description of the status value is as following.

| Code | Value      | Description |
| ---- | ---------- | ----------- |
| 0    | `Init`     | Initialized |
| 1    | `Running`  | In Progress |
| 2    | `Complete` | Completed   |
| 3    | `Cancel`   | Canceled    |

## Conclusion

In summary, the Bonus Call feature allows for the temporary allocation of bonus amounts to users during in-progress games, with careful management to ensure that agent points are unaffected and RTP calculations remain accurate.
