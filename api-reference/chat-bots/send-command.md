# How to Call Chatbot 2.0 Methods and Refresh the Authorization Token

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Chatbot `imbot.v2` methods are called in the same way as other REST API methods, but the authorization method changes the request parameter composition and determines whether the access token needs to be refreshed. The rules below apply only to `imbot.v2`: calling deprecated methods `imbot.*` is described on the [How to Call Deprecated Chatbot Methods](./outdated/send-command.md) page.

To call methods on behalf of a bot, register the bot using the [imbot.v2.Bot.register](./chat-bots-v2/imbot.v2/bots/bot-register.md) method. For an OAuth scenario, the application must be installed in Bitrix24: Bitrix24 issues the first token pair upon installation. How to retrieve and retain this pair is described in [Application Installation Scenarios](../../settings/app-installation/index.md).

Parameters, responses, and error codes for individual methods are described on the method pages in the [Chatbots 2.0](./chat-bots-v2/index.md) section. If messages need to be sent on behalf of a user rather than a bot, use the `im.*` methods in the [Chats](../chats/index.md) section — authorization in them is structured differently, and `botToken` is not involved.

> Scope: [`imbot`](../scopes/permissions.md)
>
> Who can execute the methods: the owner of the registered bot — the application or webhook on behalf of which the bot was registered. Exceptions: [imbot.v2.Bot.register](./chat-bots-v2/imbot.v2/bots/bot-register.md) — an authorized user, [imbot.v2.Revision.get](./chat-bots-v2/imbot.v2/revision-get.md) — any user

## Which Tokens Are Involved in the Call {#tokens}

Four different tokens are encountered in bot calls. Only `access_token` must be refreshed.

#|
|| **Token** | **Where it is passed** | **Where it comes from** | **Lifespan** ||
|| Webhook code — denoted as `{webhook_token}` in URL schemes | In the request path: `/rest/{user_id}/{webhook_token}/{method}`, where `{user_id}` is the ID of the user who created the webhook | Created in the Bitrix24 interface when setting up an [incoming webhook](../../local-integrations/local-webhooks.md) | Valid until the webhook is deleted ||
|| `botToken` | As a request parameter along with `botId` | Set by you in `fields.botToken` when registering a bot using the [imbot.v2.Bot.register](./chat-bots-v2/imbot.v2/bots/bot-register.md) method | Valid until you change it using the [imbot.v2.Bot.update](./chat-bots-v2/imbot.v2/bots/bot-update.md) method ||
|| `access_token` | As the `auth` parameter | Issued by the [OAuth server](../../settings/oauth/index.md) upon application installation and again with each [token pair refresh](#refresh). An application with a UI receives a ready-to-use token in the `AUTH_ID` parameter upon each opening — this is a [simplified way to obtain tokens](../../settings/oauth/simple-way.md) | One hour ||
|| `refresh_token` | Not passed in method calls — only in the request to refresh the token pair | Issued by the OAuth server along with `access_token` | 180 days ||
|#

`botToken` is not an OAuth token; it does not expire and is not involved in authorization renewal. It identifies the bot during a webhook call: Bitrix24 uses it to determine the bot owner instead of the `client_id` application.

## How to Choose an Authorization Method {#auth-modes}

#|
|| **Criterion** | **Incoming webhook** | **OAuth** ||
|| When to use | Local integration, AI agent, testing within a single Bitrix24 | Market application or an internal application working across multiple Bitrix24 instances ||
|| Request format | `POST https://{portal}/rest/{user_id}/{webhook_token}/{method}` | `POST https://{portal}/rest/{method}` with parameter `auth`. The token can also be passed in the query string — `?auth={access_token}`, and in the request body ||
|| `botToken` parameter | Mandatory for all `imbot.v2` methods, except for [imbot.v2.Revision.get](./chat-bots-v2/imbot.v2/revision-get.md). In [imbot.v2.Bot.register](./chat-bots-v2/imbot.v2/bots/bot-register.md), it is passed inside `fields.botToken`, in other methods — as a top-level parameter | Not needed: the bot is linked to the application via `client_id` ||
|| Token refresh | Not required | Required when `access_token` has expired ||
|#

Calls via an incoming webhook are performed only via the HTTPS protocol: if accessed via HTTP, an error `INVALID_REQUEST` with the description `Https required` will be returned. Such calls are performed with the permissions of the user who created the webhook and within the scope selected for the webhook.

A detailed description of both methods is available in the [Authorization](./chat-bots-v2/index.md#auth) section.

## Basic Method Call {#basic-call}

Below is a call to the [imbot.v2.Chat.Message.send](./chat-bots-v2/imbot.v2/messages/chat-message-send.md) method in two cURL tabs, one for each authorization method, and via the ready-to-use PHP CRest wrapper.

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"botId":456,"botToken":"my_bot_token","dialogId":"chat5","fields":{"message":"Enter search string"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Chat.Message.send
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"botId":456,"dialogId":"chat5","fields":{"message":"Enter search string"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imbot.v2.Chat.Message.send
  ```

- PHP CRest

  ```php
  require_once('crest.php');

  $result = CRest::call(
      'imbot.v2.Chat.Message.send',
      [
          'botId' => 456,
          // 'botToken' => 'my_bot_token', // only during webhook authorization
          'dialogId' => 'chat5',
          'fields' => [
              'message' => 'Enter search string',
          ],
      ]
  );

  if (!empty($result['error'])) {
      echo 'Error: ' . $result['error_description'];
  } else {
      echo 'Message ID: ' . $result['result']['id'];
  }
  ```

{% endlist %}

`botId` is returned by the [imbot.v2.Bot.register](./chat-bots-v2/imbot.v2/bots/bot-register.md) method upon bot registration. The `dialogId` format is `chat{chatId}` for group chats and `{userId}` for private chats; for more details, see: [dialogId Format](./chat-bots-v2/index.md#dialog-id).

Examples of the same call in JS, PHP, and BX24.js can be found in the "Code Examples" section on the [imbot.v2.Chat.Message.send](./chat-bots-v2/imbot.v2/messages/chat-message-send.md) method page.

{% note warning "" %}

Ready-to-use wrappers do not automatically insert `botToken` — when using webhook authorization, add it to the call parameters yourself.

{% endnote %}

### What the Call Returns {#response}

The response of any method consists of a `result` block containing the method data and a `time` service block containing the request execution time. For `imbot.v2.Chat.Message.send`, the `result` block looks like this:

```json
{
    "result": {
        "id": 789,
        "uuidMap": {}
    }
}
```

The composition of `result` is unique to each method and is described in the "Returned Data" section on its page.

## Authorization Errors {#auth-errors}

#|
|| **Code** | **Reason** | **What to do** ||
|| `BOT_TOKEN_NOT_SPECIFIED` | Webhook call without the `botToken` parameter | Pass the `botToken` specified during bot registration ||
|| `BOT_OWNERSHIP_ERROR` | A bot with the specified `botId` belongs to another application or was registered with a different `botToken` | Call the bot methods from the same application that registered it, or pass the same `botToken` that was specified during registration ||
|| `expired_token` | `access_token` has expired | Update the token pair and retry the request; the procedure is described in the [OAuth token update](#refresh) section ||
|| `NO_AUTH_FOUND` | Invalid `access_token` or webhook code | Check authorization data ||
|| `insufficient_scope` | The token does not have `imbot` permissions | Add the `imbot` scope to the application or webhook settings and re-authorize ||
|#

System errors `expired_token` and `NO_AUTH_FOUND` are returned with status `401`, while `insufficient_scope` is returned with status `403`. Full list: [Error Codes](../../error-codes.md).

## OAuth Token Refresh {#refresh}

This section applies only to the OAuth scenario.

### When to Refresh the Token {#when-to-refresh}

Refresh the token upon an error rather than on a schedule:

1. Call the method using the saved `access_token`.
2. If an error `expired_token` is returned with status `401` — request a new token pair using the saved `refresh_token`.
3. Retain the new token pair on your side: along with `access_token`, the server returns a new `refresh_token` value; use this value moving forward.
4. Retry the original request with the new `access_token`.

If the authorization server does not return a new pair, it means the `refresh_token` itself has expired or the application has been removed from Bitrix24. Authorization cannot be restored via a request — the application must be reinstalled, and the tokens issued during installation must be retained.

{% note alert "" %}

Do not refresh the token preemptively — before every call, once an hour, or on a schedule. This creates unnecessary load on the authorization server, which may cause the application to be blocked by automated systems. For more details, see: [OAuth 2.0 Automatic Token Renewal](../../settings/oauth/auto-renewal.md)

{% endnote %}

### How to Refresh the Token {#refresh-tools}

Ready-made wrappers handle the refresh process automatically:

- [PHP CRest](../../sdk/crest-php-sdk/index.md) — a set of PHP files for your web server; requires the cURL module. It renews tokens automatically and manages their storage itself.
- [b24phpsdk](../../sdk/b24phpsdk/index.md) — a Composer package with typed services; requires PHP 8.2 or higher. It updates an expired `access_token` and notifies you via the `AuthTokenRenewedEvent` event; the developer is responsible for implementing the storage of the new pair.
- [b24jssdk](../../sdk/b24jssdk/index.md) — a JavaScript library installed via npm. It updates the pair upon an error `expired_token`; the developer is responsible for implementing the storage of the new pair.

The `restAuth` function below is required when you call the Bitrix24 REST API using your own code without a wrapper.

### The restAuth Function {#rest-auth}

The function exchanges the saved `refresh_token` for a new token pair. The constants `CLIENT_ID` and `CLIENT_SECRET` are the application code and secret key from the partner portal or from the local application card in Bitrix24.

The refresh is performed via a GET request to the authorization server with four parameters in the query string: `grant_type=refresh_token`, `client_id`, `client_secret`, and `refresh_token`. The server address depends on the license region and is returned in the `domain` field of the token request response. If the application operates in multiple regions, retrieve the host from the `domain` field rather than from a constant.

```php
const OAUTH_SERVER = 'https://oauth.bitrix.info/oauth/token/';
const CLIENT_ID = '**put_your_client_id_here**';
const CLIENT_SECRET = '**put_your_client_secret_here**';

/**
 * Refresh OAuth token pair.
 *
 * @param array $auth Saved authorization data with refresh_token
 *
 * @return array|false New token pair or false if refresh failed
 */
function restAuth(array $auth)
{
    if (!CLIENT_ID || !CLIENT_SECRET || empty($auth['refresh_token']))
    {
        return false;
    }

    $queryData = http_build_query(
        [
            'grant_type' => 'refresh_token',
            'client_id' => CLIENT_ID,
            'client_secret' => CLIENT_SECRET,
            'refresh_token' => $auth['refresh_token'],
        ]
    );

    $curl = curl_init();

    curl_setopt_array(
        $curl,
        [
            CURLOPT_HEADER => 0,
            CURLOPT_RETURNTRANSFER => 1,
            CURLOPT_URL => OAUTH_SERVER . '?' . $queryData,
        ]
    );

    $result = curl_exec($curl);
    curl_close($curl);

    $tokens = json_decode($result, true);

    return empty($tokens['access_token']) ? false : $tokens;
}
```

The server returns JSON, from which you must retain four fields:

- `access_token` — the new access token for the `auth` parameter
- `refresh_token` — the new value; do not use the old one anymore
- `expires_in` — the lifetime of the `access_token` in seconds
- `domain` — the authorization server domain for the next refresh

The full structure of the authorization server response is described on the [OAuth 2.0 Automatic Token Renewal](../../settings/oauth/auto-renewal.md) page.

### How to Use restAuth {#rest-auth-usage}

`callRest` in the example is your method call function, which passes `access_token` into the `auth` parameter.

```php
$params = [
    'botId' => 456,
    'dialogId' => 'chat5',
    'fields' => ['message' => 'Enter search string'],
];

$result = callRest('imbot.v2.Chat.Message.send', $params, $auth['access_token']);

if (($result['error'] ?? '') === 'expired_token')
{
    $newAuth = restAuth($auth);

    if ($newAuth === false)
    {
        // refresh_token expired or application deleted — reinstallation of the application is required
        error_log('Token refresh failed');
    }
    else
    {
        $auth = $newAuth;
        saveAuth($auth); // save the new token pair in your storage
        $result = callRest('imbot.v2.Chat.Message.send', $params, $auth['access_token']);
    }
}
```

## Secret Storage {#secrets}

Consider the webhook code along with its URL, `CLIENT_SECRET`, `refresh_token`, and `botToken` as secrets:

- Store them only on your server — the webhook code provides full access to the Bitrix24 REST API within its scope and does not require separate confirmation
- Do not pass them to the browser, links, or application logs
- Do not save them in a repository — move them to environment variables or a secure data store

General integration security rules can be found in [Security Recommendations](../../settings/cloud-and-on-premise/security-recommendations.md).

## Common Sources of Confusion {#pitfalls}

Two places where similar names mean different things:

- In `fields.*`, parameters describing the content — message text, bot properties, command configurations — are nested. Identifiers and service parameters such as `botId`, `dialogId`, `botToken`, and `offset` are passed at the top level of the request
- `eventMode` of a bot is not linked to the authorization method: a bot with webhook authorization can operate in `fetch` mode, and the OAuth application bot is in mode `webhook`. The event delivery modes themselves are described in the [Event Delivery Modes](./chat-bots-v2/index.md#event-modes) section

## Continue Learning

- [{#T}](./chat-bots-v2/quick-start.md) — the first bot from registration to responding to a message
- [{#T}](./chat-bots-v2/index.md) — all methods in the section, bot types, limits, and the `dialogId` format
- [{#T}](./chat-bots-v2/imbot.v2/bots/bot-register.md) — where `botToken` is defined and `eventMode` is selected
- [{#T}](./chat-bots-v2/imbot.v2/messages/chat-message-send.md) — parameters, responses, and method error codes from the examples
- [{#T}](../../settings/oauth/auto-renewal.md) — the full OAuth token lifecycle
- [How to Call Deprecated Chatbot Methods](./outdated/send-command.md) — only for integrations using deprecated `imbot.*` methods
