# How to Call Methods of Deprecated Chatbots and Update the Authorization Token

This page outlines the basic approach to calling REST methods of deprecated chatbots and explains when to update the OAuth token and when webhook authorization is sufficient.

{% note warning "DEPRECATED" %}

For new integrations, use the methods [`imbot.v2`](../chat-bots-v2/index.md).

{% endnote %}

## Basic Method Call

Below is a typical example of calling the method [imbot.message.add](./messages/imbot-message-add.md) in standard formats used in the documentation.

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"BOT_ID":39,"DIALOG_ID":"chat123","MESSAGE":"Enter search string","CLIENT_ID":"**put_your_client_id_here**"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.message.add
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"BOT_ID":39,"DIALOG_ID":"chat123","MESSAGE":"Enter search string","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imbot.message.add
  ```

- JS

  ```js
  try {
      const response = await $b24.callMethod('imbot.message.add', {
          BOT_ID: 39,
          DIALOG_ID: 'chat123',
          MESSAGE: 'Enter search string',
      });

      const result = response.getData().result;
      console.log('Created message ID:', result);
  } catch (error) {
      console.error('Error:', error);
  }
  ```

- PHP

  ```php
  try {
      $response = $b24Service
          ->core
          ->call(
              'imbot.message.add',
              [
                  'BOT_ID' => 39,
                  'DIALOG_ID' => 'chat123',
                  'MESSAGE' => 'Enter search string',
              ]
          );

      $result = $response
          ->getResponseData()
          ->getResult();

      echo 'Created message ID: ' . $result;
  } catch (Throwable $e) {
      error_log($e->getMessage());
      echo 'Error: ' . $e->getMessage();
  }
  ```

- BX24.js

  ```js
  BX24.callMethod(
      'imbot.message.add',
      {
          BOT_ID: 39,
          DIALOG_ID: 'chat123',
          MESSAGE: 'Enter search string',
      },
      function(result) {
          if (result.error()) {
              console.error(result.error().ex);
          } else {
              console.log('Message ID:', result.data());
          }
      }
  );
  ```

- PHP CRest

  ```php
  require_once('crest.php');

  $result = CRest::call(
      'imbot.message.add',
      [
          'BOT_ID' => 39,
          'DIALOG_ID' => 'chat123',
          'MESSAGE' => 'Enter search string',
      ]
  );

  if (!empty($result['error'])) {
      echo 'Error: ' . $result['error_description'];
  } else {
      echo 'Message ID: ' . $result['result'];
  }
  ```

{% endlist %}

{% note info "" %}

If you are using your own PHP wrapper for REST, it can replicate the logic of the standard examples above: forming the HTTP request, passing method parameters, and, if necessary, adding OAuth authorization.

{% endnote %}

## Calling Scenarios

There are two main scenarios for calling methods:

1. Incoming Webhook
2. OAuth Authorization

### Incoming Webhook

If you are calling methods via an incoming webhook, there is no need to update the OAuth token. For methods of deprecated chatbots in the webhook scenario, the `CLIENT_ID` specified during bot registration is also passed.

### OAuth

If you are calling REST via OAuth, the request includes an `access_token` and a `refresh_token`. In this case, the access token may expire, and it needs to be refreshed using the `refresh_token`.

For this scenario, the `restAuth` function is useful.

#### `restAuth` Function

Use this function only for the OAuth scenario.

```php
/**
 * Refresh OAuth token.
 *
 * @param array $auth OAuth authorization data
 *
 * @return bool|array
 */
function restAuth($auth)
{
    if (!CLIENT_ID || !CLIENT_SECRET)
    {
        return false;
    }

    if (
        !isset($auth['refresh_token'])
        || !isset($auth['scope'])
        || !isset($auth['domain'])
    )
    {
        return false;
    }

    $queryUrl = 'https://' . $auth['domain'] . '/oauth/token/';
    $queryData = http_build_query(
        [
            'grant_type' => 'refresh_token',
            'client_id' => CLIENT_ID,
            'client_secret' => CLIENT_SECRET,
            'refresh_token' => $auth['refresh_token'],
            'scope' => $auth['scope'],
        ]
    );

    $curl = curl_init();

    curl_setopt_array(
        $curl,
        [
            CURLOPT_HEADER => 0,
            CURLOPT_RETURNTRANSFER => 1,
            CURLOPT_URL => $queryUrl . '?' . $queryData,
        ]
    );

    $result = curl_exec($curl);
    curl_close($curl);

    return json_decode($result, true);
}
```

## Considerations for Deprecated Chatbots

- For webhook method calls, pass `CLIENT_ID`
- For OAuth calls, `CLIENT_ID` is not needed, but `auth` is required
- Use `imbot.message.*` for sending messages, `imbot.chat.*` for chats, and `imbot.command.*` for commands

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./messages/imbot-message-add.md)
- [{#T}](./bots/imbot-register.md)
- [{#T}](../send-command.md)
- [{#T}](../../../tutorials/chat-bots/index.md)