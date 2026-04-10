# Get the List of Connectors imconnector.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify Open Channels connectors

The method `imconnector.list` returns a list of all connectors registered in Bitrix24.

{% note info "" %}

The method works only in the context of an [application](../../../settings/app-installation/index.md).

{% endnote %} 

## Method Parameters

No parameters.

## Code Examples

{% include [Example Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imconnector.list
    ```

- JS

    ```js
    const response = await $b24.callMethod('imconnector.list', {});
    console.log(response.getData());
    ```

- PHP

    ```php
    $result = $b24Service->core->call(
        'imconnector.list',
        []
    );
    ```

- BX24.js

    ```js
    BX24.callMethod(
      'imconnector.list',
      {},
      function(result) {
        console.log(result.data());
      }
    );
    ```

- PHP CRest

    ```php
    $result = CRest::call(
        'imconnector.list',
        []
    );
    ```
{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "livechat": "Live Chat",
        "telegrambot": "Telegram",
        "network": "Bitrix24.Network",
        "myconnector": "My Connector"
    },
    "time": {
        "start": 1738065600.11,
        "finish": 1738065600.17,
        "duration": 0.06,
        "processing": 0.03,
        "date_start": "2025-01-28T12:00:00+00:00",
        "date_finish": "2025-01-28T12:00:00+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | An object of the form `connector_id: connector_name` for available connectors ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "You don't have access to this action"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method. Application context required | Method called outside of the application OAuth context ||
|| `400` | `ACCESS_DENIED` | The ImOpenLines module is not installed | The `imopenlines` module is not installed on the account ||
|| `400` | `ACCESS_DENIED` | You don't have access to this action | The user does not have permission to modify connectors ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imconnector-register.md)
- [{#T}](./imconnector-activate.md)
- [{#T}](./imconnector-status.md)
- [{#T}](./imconnector-connector-data-set.md)
- [{#T}](./imconnector-unregister.md)
- [{#T}](./imconnector-send-messages.md)
- [{#T}](./imconnector-update-messages.md)
- [{#T}](./imconnector-delete-messages.md)
- [{#T}](./imconnector-send-status-delivery.md)
- [{#T}](./imconnector-chat-name-set.md)