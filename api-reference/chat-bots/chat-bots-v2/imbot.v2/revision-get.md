# Get API Revisions imbot.v2.Revision.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.v2.Revision.get` returns the revision numbers of the REST API and client protocols of the messenger. It is used to check compatibility: which methods and features are supported by a specific Bitrix24.

## Purpose of the Method

Cloud and on-premise versions of Bitrix24 may have different API revisions. Cloud Bitrix24 instances are updated automatically, while on-premise installations may lag behind in capabilities.

By calling `imbot.v2.Revision.get` before using new methods or fields, the application can:

- determine which features are available on the current Bitrix24
- adapt the bot's logic to the API revision
- correctly handle scenarios where the required functionality is not yet available to the client

In the method documentation, you may see the note **“available from revision N”**. This means that the field or behavior was introduced starting from the specified revision.

## Method Parameters

The method does not require `botId` and `botToken`. There are no parameters.

## How to Use

A typical scenario is to check before using a method or field that appeared in a specific revision:

```js
const revision = await BX.rest.callMethod('imbot.v2.Revision.get', {});
const restRevision = revision.data().rest;

if (restRevision >= 33)
{
    await BX.rest.callMethod('imbot.v2.Chat.Message.send', {
        botId: 456,
        botToken: '...',
        dialogId: 'chat5',
        fields: { message: 'Hello', system: true }
    });
}
else
{
    // system may not work correctly in earlier revisions
}
```

## Code Examples

{% include [Example Notes](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Revision.get
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imbot.v2.Revision.get
  ```

- JS

  ```js
  BX.rest.callMethod('imbot.v2.Revision.get', {})
      .then(result => console.log(result.data()));
  ```

- PHP

  ```php
  $result = $b24Service->core->call('imbot.v2.Revision.get');
  print_r($result->getResponseData()->getResult());
  ```

- BX24.js

  ```js
  BX24.callMethod('imbot.v2.Revision.get', {}, function(result) {
      if (result.error()) {
          console.error(result.error().ex);
      } else {
          console.log(result.data());
      }
  });
  ```

- PHP CRest

  ```php
  $result = CRest::call('imbot.v2.Revision.get');
  print_r($result['result']);
  ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
  "result": {
    "rest": 33,
    "web": 130,
    "mobile": 23,
    "desktop": 6
  },
  "time": {
    "start": 1728626400.123,
    "finish": 1728626400.234,
    "duration": 0.111,
    "processing": 0.045,
    "date_start": "2024-10-11T10:00:00+02:00",
    "date_finish": "2024-10-11T10:00:00+02:00"
  }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[object](../../../data-types.md) | API revision numbers and client protocols [(detailed description)](#revision-object) ||
|| **time**
[time](../../../data-types.md#time) | Information about the request execution time ||
|#

### Fields of the Revision Object {#revision-object}

#|
|| **Field**
`Type` | **Description** ||
|| **rest**
[`integer`](../../../data-types.md) | Revision of the server REST API. The main key for checking compatibility of methods and fields ||
|| **web**
[`integer`](../../../data-types.md) | Revision of the web client protocol of the messenger ||
|| **mobile**
[`integer`](../../../data-types.md) | Revision of the mobile client protocol ||
|| **desktop**
[`integer`](../../../data-types.md) | Revision of the desktop application protocol ||
|#

## Error Handling

The method does not return call errors. Only standard REST API authorization errors may occur.

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../index.md)
- [{#T}](../change-log.md)
- [{#T}](./bots/bot-register.md)