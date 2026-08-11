# Get API revisions imbot.v2.Revision.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

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

No parameters. The method does not require `botId` and `botToken`.

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

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
    try {
      const response = await $b24.callMethod('imbot.v2.Revision.get', {});

      const { result } = response.getData();
      console.log('result:', result);
    } catch (error) {
      console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call('imbot.v2.Revision.get');

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'result: ' . print_r($result, true);
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.v2.Revision.get',
        {},
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call('imbot.v2.Revision.get');

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'REST revision: ' . $result['result']['rest'];
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "imbot.v2.Revision.get", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("imbot.v2.Revision.get: %w", err)
    }

    var item struct {
    	Rest    int `json:"rest"`
    	Web     int `json:"web"`
    	Mobile  int `json:"mobile"`
    	Desktop int `json:"desktop"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Rest, item.Web)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

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
[`object`](../../../data-types.md) | API revision numbers and client protocols [(detailed description)](#revision-object) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
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

## Checking Compatibility Before a Call

A typical scenario is to check the revision before using a method or field that was not available from the start:

```js
const response = await $b24.callMethod('imbot.v2.Revision.get', {});
const restRevision = response.getData().result.rest;

if (restRevision >= 33) {
    // the fields.system field is supported — send a system message
    await $b24.callMethod('imbot.v2.Chat.Message.send', {
        botId: 456,
        dialogId: 'chat5',
        fields: { message: 'Hello', system: true }
    });
} else {
    // in earlier revisions, the fields.system field may be handled incorrectly
}
```

The revision number from which a specific change becomes available is listed in the [API Change Log for imbot.v2](../change-log.md).

## Error Handling

The method does not return call errors. Only standard REST API authorization errors may occur.

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../index.md)
- [{#T}](../change-log.md)
- [{#T}](./bots/bot-register.md)