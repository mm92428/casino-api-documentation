# Seamless Mode

## Basic Insights

The SlotCity API offers seamless integration, providing the flexibility to manage your users' balances directly. With Seamless Mode, you can handle deposits, withdrawals, log history, and generate statistics independently.

This approach isolates your users' balances, ensuring you are protected from any potential malicious activities by API providers.&#x20;

By using this mode, you can seamlessly integrate multiple APIs within your casino. This allows you to manage various functionalities, such as user balances and transactions, across different API providers without any conflicts. It ensures smooth operation and flexibility, enabling you to leverage the strengths of different APIs simultaneously.

In this mode, our API will send specific requests to retrieve necessary information, such as the user's balance and authority. It will also update the user's balance whenever they place a bet and win or lose, by communicating with your casino's backend and processing the results.

Here is a diagram that explains the workflow in Seamless Mode.

<figure><img src="https://3590886847-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FsQaLK3pikuFct6LKHcJX%2Fuploads%2FObCAYjVc5DqOkRaUZwoh%2FDesktop%20(1280%20_%20720).png?alt=media&#x26;token=e7ee036e-2740-43eb-9ca4-015f9dc01b3a" alt=""><figcaption><p>Seamless Mode Workflow</p></figcaption></figure>

<details>

<summary>Calling our main API's endpoint</summary>

You call our main API's endpoint (eg: `/v4/user/create` to create a new user in our API)

</details>

<details>

<summary>Calling back your casino backend</summary>

Our API will send a request to your Callback URL to obtain essential data, such as the player's balance and permissions, because we do not store player data on our servers. This type of request is referred to as a <mark style="color:red;">'Callback Request'</mark> in this document.

When our API calls back your backend, we send a specific header value named `Callback-Token`. Callback Token string is issued by our API. You can find this in <mark style="color:blue;">\[Settings]</mark> page within our back office.

For more information, please see [Callback Requests](https://slotcity.gitbook.io/slotcity-api-documentation/api-reference/images-and-media) page.&#x20;

</details>

<details>

<summary>Respond to our callback request</summary>

You must respond to our Callback Request with the required data.

Before sending your valuable result, please verify the `Callback-Token` header value from our callback request and validate all data according to our check requests.

<mark style="color:red;">**The data structure is crucial for our API, as we need to parse your response according to predefined rules.**</mark>

For more information, please see [Callback Responses](https://slotcity.gitbook.io/slotcity-api-documentation/api-reference/interactive-blocks) page.

</details>

<details>

<summary>Our API returns final result</summary>

After receiving your response, we parse the data in the response body and determine whether to send a success message or an error message.

You can see our API's responses, please see [Main API](https://slotcity.gitbook.io/slotcity-api-documentation/api-reference/openapi) page.

</details>

## Integration Roadmap

***

### Step 1. Setting a Callback URL&#x20;

Callback URL is an endpoint that our API can access over the internet and send callback requests.

To set a callback URL, navigate to the <mark style="color:blue;">\[Settings]</mark> page within our back office system. This is where you can configure various settings related to your account and operations.

The callback URL you enter must begin with either http\:// or https\://. This ensures that the URL is correctly formatted and can be accessed over the web. For example, a valid callback URL might look like this: `https://test.com/callback`.

After you set the callback URL, it may take up to 10 minutes for the changes to be applied to the real server. This delay allows the system to properly update and ensure that the new URL is correctly integrated.

If you are using the transfer wallet mode, you should leave the Callback URL field empty. This is because the transfer wallet mode does not require a callback URL for its operations.

***

### Step 2. Referencing a Callback Processing Code Example

You can learn how to implement callback processing code by referring to our [Seamless Integration Code Examples](https://slotcity.gitbook.io/slotcity-api-documentation/api-reference/integrations).

***

### Step 3. Writing a Callback Processing Codes

After referring to the [Seamless Integration Code Examples](https://slotcity.gitbook.io/slotcity-api-documentation/api-reference/integrations), you must implement all functions.

At this stage, you need to implement server-side code to handle our callback requests. Specifically, you should create an endpoint using the Callback URL that can listen for incoming callback requests from our system. This endpoint should be capable of parsing the callback request data and responding with the appropriate information or error message.

In summary:

1. **Create an Endpoint**: Develop an endpoint on your server that matches the Callback URL.
2. **Listen for Requests**: Ensure this endpoint is set up to listen for incoming callback requests from our system.
3. **Parse the Request**: Write code to parse the data contained in the callback request.
4. **Respond Appropriately**: Based on the parsed data, return the necessary information or an error message if something goes wrong.

By following these steps, you can effectively handle our callback requests and ensure smooth communication between our systems.

{% hint style="warning" %}
You are responsible for any non-implementation.
{% endhint %}

You can review our callback request specifications on the [Callback Requests](https://slotcity.gitbook.io/slotcity-api-documentation/api-reference/images-and-media) page. Additionally, you can learn how to respond to our callback requests on the [Callback Responses](https://slotcity.gitbook.io/slotcity-api-documentation/api-reference/interactive-blocks) page.

When writing the callback processing code, please refer to the <mark style="color:blue;">\[API Error Logs]</mark> page in our back office. You can view all errors on this page and debug your code based on these error logs.

{% hint style="danger" %}
Variable Type Caution! Please refer to the example codes and develop it according to `string` and `int` variable types.

In the case of string, it is marked with single quotation marks, such as `'test'`, and in the case of balance, it must be matched with `int` type, such as `1000`.
{% endhint %}

***

### Step 4. Callback Processing Test

Begin by navigating to the <mark style="color:blue;">\[API/Callback API Testing]</mark> page within our back office system. This page is specifically designed to guide you through the testing process in a structured manner. Follow each step as outlined on the page to ensure that all aspects of the callback API are thoroughly tested.

During the testing phase, you will be working with virtual data. This means that you do not need to use actual points from your wallet. The virtual data will simulate real transactions and scenarios, allowing you to test the functionality of the callback API without impacting your actual point balance.

If you find that you need actual test points for a more realistic testing scenario, you will need to obtain approval from your agent first. Once you have received this approval, you can then submit a request for the necessary test points. This ensures that all test points are allocated appropriately and with proper authorization.

{% hint style="danger" %}
Please operate after passing all tests. You are responsible for any non-implementation.
{% endhint %}

***

### **Step 5. Game Testing**

Lastly, please access API in the [Main API](https://slotcity.gitbook.io/slotcity-api-documentation/api-reference/openapi) to test the actual game.

\\
