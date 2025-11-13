# Seamless Integration Code Examples

These code examples encompass various types of callback requests and response data structures. Please review them thoroughly.

To handle our callback request, you need to:

* Create a route (callback URL)
* Parse the incoming request
* Respond with the appropriate data

Please implement the callback processing code on your server side.

{% hint style="info" %}
These code examples are for demonstration purposes only. Please do not use them in your production environment.
{% endhint %}

{% hint style="danger" %}
Please pay close attention to the response JSON structure. Many developers overlook this and end up encountering `'CALLBACK_ERROR'`.
{% endhint %}

{% tabs %}
{% tab title="Node.js" %}

```javascript
/******

VERSION : V200
Callback Handling
v200
Creating Basic Sample Source Written with Node.js (Express + MySQL)
!!! PLEASE DO NOT USE THIS EXAMPLE CODE FOR YOUR PRODUCTION, THIS IS JUST ONLY FOR YOUR REFERRENCE. !!!

/***

Example Tables
/
/*
CREATE TABLE bet_casino (
trans_id VARCHAR(64) NOT NULL,
user_id VARCHAR(20) NOT NULL,
game_id VARCHAR(100) NOT NULL DEFAULT '',
round_id VARCHAR(64) NOT NULL DEFAULT '',
sort ENUM('BET','WIN','CANCEL') NOT NULL,
money INT(11) NOT NULL,
request_datetime INT(11) NOT NULL DEFAULT '0',
PRIMARY KEY (trans_id),
INDEX game_id (game_id, round_id)
);

CREATE TABLE user_casino (
user_id VARCHAR(20) NOT NULL,
user_name VARCHAR(20) NOT NULL,
user_nickname VARCHAR(20) NOT NULL,
money INT(11) NOT NULL DEFAULT '0',
token VARCHAR(50) NOT NULL DEFAULT '',
status ENUM('Active','Drop') NOT NULL DEFAULT 'Active',
PRIMARY KEY (user_id),
INDEX token (token)
);

*/
const express = require('express');
const mysql = require('mysql2/promise'); // Using promise-based mysql2
const bodyParser = require('body-parser');

// Constants
const CALLBACK_TOKEN = 'eadd5a04-4720-4a10-ae60-b11cd01cc3aa';

// Create Express app
const app = express();
app.use(bodyParser.json());

// MySQL Database Connection
const db = mysql.createPool({
    host: '127.0.0.1',
    user: 'root',
    password: '',
    database: 'casino',
});

// Helper function to send JSON response and end connection
const sendResponse = (res, data) => {
    res.json(data);
};

// Middleware to validate Callback Token
app.use((req, res, next) => {
    const tokenData = req.headers['callback-token']?.trim();
    if (CALLBACK_TOKEN !== tokenData) {
        return sendResponse(res, { result: 100, status: 'ERROR' });
    }
    next();
});

// Main route to handle the request
app.post('/callback', async (req, res) => {
    const oData = req.body;
    const command = oData.command;
    const aCheckItem = oData.check.split(',');

    let userInfo = null;
    let betInfo = null;

    // Helper to process checklist items
    const processCheckItems = async () => {
        for (const check of aCheckItem) {
            switch (parseInt(check)) {
                case 21: // user confirmation
                    userInfo = await getUserInfo(oData.data.account);
                    if (!userInfo) return { result: check, status: 'ERROR' };
                    break;

                case 22: // Check if user is active
                    if (userInfo.status !== 'Active') return { result: check, status: 'ERROR' };
                    break;

                case 31: // Check user balance
                    const amount = parseInt(oData.data.amount);
                    if (userInfo.money < amount) {
                        return { result: check, status: 'ERROR', data: { balance: userInfo.money } };
                    }
                    break;

                case 41: // Check if trans_id exists
                    betInfo = await getBetInfo(oData.data.trans_id);
                    if (betInfo) {
                        return { result: check, status: 'ERROR', data: { balance: userInfo.money } };
                    }
                    break;

                case 42: // Check if trans_id exists (return bet info)
                    betInfo = await getBetInfo(oData.data.trans_id);
                    if (!betInfo) {
                        return { result: check, status: 'ERROR', data: { balance: userInfo.money } };
                    }
                    break;

                default:
                    break;
            }
        }
        return null;
    };

    // Handle different commands
    const handleCommand = async () => {
        let response = null;
        switch (command) {
            case 'authenticate':
                response = { result: 0, status: 'OK', data: { account: userInfo.user_id, balance: userInfo.money } };
                break;

            case 'balance':
                response = { result: 0, status: 'OK', data: { balance: userInfo.money } };
                break;

            case 'bet':
                response = await handleBet(oData);
                break;

            case 'win':
                response = await handleWin(oData);
                break;

            case 'cancel':
                response = await handleCancel(oData);
                break;

            case 'status':
                response = { result: 0, status: 'OK', data: { trans_id: oData.data.trans_id, trans_status: betInfo.sort === 'CANCEL' ? 'CANCELED' : 'OK' } };
                break;

            default:
                response = { result: 99, status: 'UNKNOWN_COMMAND' };
        }
        return response;
    };

    const checkError = await processCheckItems();
    if (checkError) return sendResponse(res, checkError);

    const result = await handleCommand();
    sendResponse(res, result);
});

// Helper functions
const getUserInfo = async (userId) => {
    const sql = 'SELECT * FROM user_casino WHERE user_id = ?';
    const [rows] = await db.query(sql, [userId]);
    return rows[0];
};

const getBetInfo = async (transId) => {
    const sql = 'SELECT * FROM bet_casino WHERE trans_id = ?';
    const [rows] = await db.query(sql, [transId]);
    return rows[0];
};

const handleBet = async (oData) => {
    const { trans_id, game_code, round_id, amount } = oData.data;
    const user_id = userInfo.user_id;
    const transTimestamp = Math.floor(Date.now() / 1000); // UNIX timestamp

    try {
        const betSql = 'INSERT INTO bet_casino (trans_id, user_id, game_id, round_id, sort, money, request_datetime) VALUES (?, ?, ?, ?, ?, ?, ?)';
        await db.query(betSql, [trans_id, user_id, game_code, round_id, 'BET', amount, transTimestamp]);

        const newBalance = userInfo.money - amount;
        const updateUserSql = 'UPDATE user_casino SET money = ? WHERE user_id = ?';
        await db.query(updateUserSql, [newBalance, user_id]);

        return { result: 0, status: 'OK', data: { balance: newBalance } };
    } catch (err) {
        return { result: 99, status: 'ERROR', data: { balance: userInfo.money } };
    }
};

const handleWin = async (oData) => {
    const { trans_id, game_code, round_id, amount } = oData.data;
    const user_id = userInfo.user_id;
    const transTimestamp = Math.floor(Date.now() / 1000); // UNIX timestamp

    try {
        const winSql = 'INSERT INTO bet_casino (trans_id, user_id, game_id, round_id, sort, money, request_datetime) VALUES (?, ?, ?, ?, ?, ?, ?)';
        await db.query(winSql, [trans_id, user_id, game_code, round_id, 'WIN', amount, transTimestamp]);

        const newBalance = userInfo.money + amount;
        const updateUserSql = 'UPDATE user_casino SET money = ? WHERE user_id = ?';
        await db.query(updateUserSql, [newBalance, user_id]);

        return { result: 0, status: 'OK', data: { balance: newBalance } };
    } catch (err) {
        return { result: 99, status: 'ERROR', data: { balance: userInfo.money } };
    }
};

const handleCancel = async (oData) => {
    const { trans_id } = oData.data;
    const user_id = userInfo.user_id;
    let user_money = userInfo.money;

    if (betInfo.sort !== 'CANCEL') {
        const isBet = betInfo.sort === 'BET';
        const moneyChange = isBet ? betInfo.money : -betInfo.money;

        try {
            const updateBetSql = 'UPDATE bet_casino SET sort = "CANCEL" WHERE trans_id = ?';
            await db.query(updateBetSql, [trans_id]);

            user_money += moneyChange;
            const updateUserSql = 'UPDATE user_casino SET money = ? WHERE user_id = ?';
            await db.query(updateUserSql, [user_money, user_id]);

            return { result: 0, status: 'OK', data: { balance: user_money } };
        } catch (err) {
            return { result: 99, status: 'ERROR', data: { balance: userInfo.money } };
        }
    } else {
        return { result: 0, status: 'OK', data: { balance: user_money } };
    }
};

```

{% endtab %}

{% tab title="PHP" %}

```php
<?php
/******
 * VERSION : V100
 * Callback Handling
 *
 * v100
 *  Creating Basic Sample Source
 */

/****
 * $requestData = file_get_contents('php://input');
 * Data Example
 * ************************************************
 *
 */

/***
 * Tables Used
 */
/**
 *

CREATE TABLE `bet_casino` (
`trans_id` VARCHAR(64) NOT NULL,
`user_id` VARCHAR(20) NOT NULL,
`game_id` VARCHAR(100) NOT NULL DEFAULT '',
`round_id` VARCHAR(64) NOT NULL DEFAULT '',
`sort` ENUM('BET','WIN','CANCEL') NOT NULL,
`money` INT(11) NOT NULL,
`request_datetime` INT(11) NOT NULL DEFAULT '0',
PRIMARY KEY (`trans_id`),
INDEX `game_id` (`game_id`, `round_id`)
);

CREATE TABLE `user_casino` (
`user_id` VARCHAR(20) NOT NULL,
`user_name` VARCHAR(20) NOT NULL,
`user_nickname` VARCHAR(20) NOT NULL,
`money` INT(11) NOT NULL DEFAULT '0',
`token` VARCHAR(50) NOT NULL DEFAULT '',
`status` ENUM('Active','Drop') NOT NULL DEFAULT 'Active',
PRIMARY KEY (`user_id`),
INDEX `token` (`token`)
);

 *
 */

/***
 * Database Connection
 */

$servername = "127.0.0.1";
$username = "root";
$password = "";
$database = "casino";


// Create connection
$dbConn = new mysqli($servername, $username, $password, $database);

// Check connection
if ($dbConn->connect_error) {
    die("Connection failed: " . $dbConn->connect_error);
}



/****
 * Import Data
 */
// CALLBACK_TOKEN
const CALLBACK_TOKEN = 'eadd5a04-4720-4a10-ae60-b11cd01cc3aa';

$headers = apache_request_headers();
$token_data = trim($headers['Callback-Token']);

// Check callback token
if (CALLBACK_TOKEN != $token_data) {
    $result = [
        'result' => 100, // Returns 100 if callback token is different
        'status' => 'ERROR',
    ];

    echo json_encode($result);
    exit;
}

$requestData = file_get_contents('php://input');
$oData = json_decode($requestData);


$command = $oData->command;
$request_timestamp = $oData->request_timestamp; // In UTC

$aCheckItem = explode(',', $oData->check);

///////////////////////
// Check that there are no errors in that item
///////////////////////
// obtained from the checklist below
// User information, betting information, etc. can be used in the data processing below.
////////////////////////////////////////////////////////////

$userInfo = '';
$betInfo = '';
foreach ($aCheckItem as $check) {
    switch ($check) {
        case 21:    // user confirmation
            $user_id = $oData->data->account;

            $sql = "SELECT * FROM user_casino WHERE user_id='{$user_id}'";
            $result = $dbConn->query($sql);

            if ($result->num_rows > 0) {
                $userInfo = $result->fetch_assoc();
            }

            if (!$userInfo) {
                $aResult = [
                    'result' => $check,
                    'status' => 'ERROR',
                ];

                $dbConn->close();
                echo json_encode($aResult);
                exit;
            }
            break;

        case 22:    // Check if it is an active user
            if ($userInfo['status'] != "Active") {    // When not an active user
                $aResult = [
                    'result' => $check,
                    'status' => 'ERROR',
                ];

                $dbConn->close();
                echo json_encode($aResult);
                exit;
            }
            break;

        case 31: // Check user balance
            $amount = intval($oData->data->amount); // Betting, Hit amount (DECIMAL 18,2)

            if ($userInfo['money'] < $amount) {   // When the balance amount is less than the betting amount
                $aResult = [
                    'result' => $check,
                    'status' => 'ERROR',
                    'data' => [
                        'balance' => $userInfo['money'],
                    ],
                ];

                $dbConn->close();
                echo json_encode($aResult);
                exit;
            }
            break;

        case 41:    // Check if the [trans] has already been processed
            $trans_id = $oData->data->trans_id; // Check if [string] value is unique

            $sql = "SELECT * FROM bet_casino WHERE trans_id='{$trans_id}'";
            $result = $dbConn->query($sql);

            // All [trans] ids must be saved
            // Check whether work has been processed
            if ($result->num_rows > 0) {
                $aResult = [
                    'result' => $check,
                    'status' => 'ERROR',
                    'data' => [
                        'balance' => $userInfo['money'],
                    ],
                ];

                $dbConn->close();
                echo json_encode($aResult);
                exit;
            }
            break;

        case 42:   // Check if [trans] id exists
            $trans_id = $oData->data->trans_id;  // unique [string] value

            // All [trans] ids must be saved
            // Check whether work has been processed
            $sql = "SELECT * FROM bet_casino WHERE trans_id='{$trans_id}'";
            $result = $dbConn->query($sql);

            // All [trans] ids must be saved
            // Check whether work has been processed
            if ($result->num_rows > 0) {
                $betInfo = $result->fetch_assoc();
            } else {
                $aResult = [
                    'result' => $check,
                    'status' => 'ERROR',
                    'data' => [
                        'balance' => $userInfo['money'],
                    ],
                ];

                $dbConn->close();
                echo json_encode($aResult);
                exit;
            }
            break;
    }
}

//////////////////////
// Handle normal completion
//////////////////////
$aResult = [];
switch ($command) {
    case "authenticate":
        $aResult = [
            'result' => 0,
            'status' => 'OK',
            'data' => [
                'account' => $userInfo['user_id'],  // Unique user ID (string: 4 to 15 possible)
                'balance' => $userInfo['money'],
            ],
        ];
        break;

    case "balance":
        $aResult = [
            'result' => 0,
            'status' => 'OK',
            'data' => [
                'balance' => $userInfo['money'],
            ],
        ];
        break;

    case "bet": // Order request processing
        $trans_id = $oData->data->trans_id;  // unique [string] value
        $call_id = $oData->data->call_id;  // unique [int] value for bonus-call on progress
        $trans_timestamp = $oData->timestamp;   // trans occurrence time (UTC)
        $amount = intval($oData->data->amount); // Betting, Hit amount (DECIMAL 18,2)
        $game_id = $oData->data->game_code;   // Game ROUND ID (ID identifying related history for multiple bets, wins)
        $game_type = $oData->data->game_type;   // Type of game (baccarat, poker, etc...)
        $round_id = $oData->data->round_id; // Game ROUND Reference ID


        // record your betting history
        // The balance amount is deducted according to the user bet amount

        $user_id = $userInfo['user_id'];
        $request_datetime = time();
        $sql = "INSERT INTO bet_casino VALUES ('{$trans_id}', '{$user_id}', '{$game_id}', '{$round_id}', 'BET', '{$amount}', '{$request_datetime}')";
        $result = $dbConn->query($sql);

        if (!$result) {
            $aResult = [
                'result' => 99,
                'status' => 'ERROR',
                'data' => [
                    'balance' => $userInfo['money'],
                ],
            ];

            $dbConn->close();
            echo json_encode($aResult);
            exit;
        }


        // Save trans information
        ////////////////////////

        $user_money = $userInfo['money'] - $amount;
        $sql = "UPDATE user_casino SET money='{$user_money}' WHERE user_id='{$user_id}'";
        $result = $dbConn->query($sql);

        // All related operations succeeded
        $aResult = [
            'result' => 0,
            'status' => 'OK',
            'data' => [
                'balance' => $user_money,
            ],
        ];
        break;

    case "win":    // Handling order item hits and misses
        $trans_id = $oData->data->trans_id;  // unique [string] value
        $call_id = $oData->data->call_id;  // unique [int] value for bonus-call on progress
        $amount = intval($oData->data->amount); // Betting, Hit amount (DECIMAL 18,2)
        $game_id = $oData->data->game_code;   // Game ROUND ID (ID identifying related history for multiple bets, wins)
        $round_id = $oData->data->round_id; // Game ROUND Reference ID
        
        $user_id = $userInfo['user_id'];
        $request_datetime = time();
        $sql = "INSERT INTO bet_casino VALUES ('{$trans_id}', '{$user_id}', '{$game_id}', '{$round_id}', 'WIN', '{$amount}', '{$request_datetime}')";
        $result = $dbConn->query($sql);

        $user_money = $userInfo['money'] + $amount;
        if ($result) {
            if ($amount > 0) {
                $sql = "UPDATE user_casino SET money='{$user_money}' WHERE user_id='{$user_id}'";
                $result = $dbConn->query($sql);
            }
        } else {
            $aResult = [
                'result' => 99,
                'status' => 'ERROR',
                'data' => [
                    'balance' => $userInfo['money'],
                ],
            ];

            $dbConn->close();
            echo json_encode($aResult);
            exit;
        }


        // All related operations succeeded
        $aResult = [
            'result' => 0,
            'status' => 'OK',
            'data' => [
                'balance' => $user_money,
            ],
        ];
        break;

    case "cancel":
        $money = 0;
        $user_id = $userInfo['user_id'];
        $user_money = $userInfo['money'];

        if ($betInfo['sort'] != "CANCEL") {    // Situations requiring cancellation
            // cancel operation in progress
            
            if ($betInfo['sort'] == "BET") {   // When it matches the trans id processed by bet
                // Proceed with the bet cancellation process
                $money = $betInfo['money'];

                $sql = "UPDATE bet_casino SET sort='CANCEL' WHERE trans_id='{$trans_id}'";
                $result = $dbConn->query($sql);

                if ($result) {
                    $user_money = $userInfo['money'] + $money;
                    $sql = "UPDATE user_casino SET money='{$user_money}' WHERE user_id='{$user_id}'";
                    $result = $dbConn->query($sql);
                } else {
                    $aResult = [
                        'result' => 99,
                        'status' => 'ERROR',
                        'data' => [
                            'balance' => $userInfo['money'],
                        ],
                    ];

                    $dbConn->close();
                    echo json_encode($aResult);
                    exit;
                }
            } else if ($betInfo['sort'] == "WIN") {
                // Proceed with the result cancellation process
                $money = -1 * $betInfo['money'];

                $sql = "UPDATE bet_casino SET sort='CANCEL' WHERE trans_id='{$trans_id}'";
                $result = $dbConn->query($sql);

                if ($result) {
                    $user_money = $userInfo['money'] + $money;
                    $sql = "UPDATE user_casino SET money='{$user_money}' WHERE user_id='{$user_id}'";
                    $result = $dbConn->query($sql);
                } else {
                    $aResult = [
                        'result' => 99,
                        'status' => 'ERROR',
                        'data' => [
                            'balance' => $userInfo['money'],
                        ],
                    ];

                    $dbConn->close();
                    echo json_encode($aResult);
                    exit;
                }
            }
        }

        $aResult = [
            'result' => 0,
            'status' => 'OK',
            'data' => [
                'balance' => $user_money,
            ],
        ];
        break;

    case "status":
        $trans_id = $oData->data->trans_id;  // unique [string] value

        if ($betInfo['sort'] == "CANCEL") {    // status of cancellation
            $aResult = [
                'result' => 0,
                'status' => 'OK',
                'data' => [                    
                    'trans_id' => $trans_id,
                    'trans_status' => 'CANCELED',
                ],
            ];
        } else {
            $aResult = [
                'result' => 0,
                'status' => 'OK',
                'data' => [                    
                    'trans_id' => $trans_id,
                    'trans_status' => 'OK',
                ],
            ];
        }
        break;
}

$dbConn->close();
echo json_encode($aResult);
?>
```

{% endtab %}
{% endtabs %}
