# Get the Status of the Connector imconnector.status

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imconnector.status` returns the current status of the connector for the specified open line.

{% note info "" %}

The method works only in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %} 

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONNECTOR***
[`string`](../../data-types.md) | The string code of the connector specified in the `ID` parameter when calling [imconnector.register](./imconnector-register.md) ||
|| **LINE**
[`string`](../../data-types.md) | The identifier of the open line ||
|#

If the `LINE` parameter is not provided, the method automatically uses the value `0`. This affects the result of the check:
- For a valid line identifier, the connector may be active and configured.
- With `LINE=0`, the method typically returns `CONFIGURED=false` and `STATUS=false`, even if the connector is functioning for other lines.

To obtain an accurate status, always specify the identifier of the open line.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CONNECTOR":"myconnector","LINE":"12","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imconnector.status
    ```

- JS

    ```js
    const response = await $b24.callMethod('imconnector.status', {
      CONNECTOR: 'myconnector',
      LINE: '12',
    });
    console.log(response.getData());
    ```

- PHP

    ```php
    $result = $b24Service->core->call(
        'imconnector.status',
        [
            'CONNECTOR' => 'myconnector',
            'LINE' => '12',
        ]
    );
    ```

- BX24.js

    ```js
    BX24.callMethod(
      'imconnector.status',
      {
        CONNECTOR: 'myconnector',
        LINE: '12',
      },
      function(result) {
        console.log(result.data());
      }
    );
    ```

- PHP CRest

    ```php
    $result = CRest::call(
        'imconnector.status',
        [
            'CONNECTOR' => 'myconnector',
            'LINE' => '12',
        ]
    );
    ```
{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "LINE": 12,
        "CONNECTOR": "myconnector",
        "ERROR": false,
        "CONFIGURED": true,
        "STATUS": true
    },
    "time": {
        "start": 1738065600.11,
        "finish": 1738065600.18,
        "duration": 0.07,
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
|| **LINE**
[`integer`](../../data-types.md) | The identifier of the open line ||
|| **CONNECTOR**
[`string`](../../data-types.md) | The identifier of the connector ||
|| **ERROR**
[`boolean`](../../data-types.md) | Indicates an error in the connector's status ||
|| **CONFIGURED**
[`boolean`](../../data-types.md) | Indicates whether the connector is fully configured ||
|| **STATUS**
[`boolean`](../../data-types.md) | The final status of the connector's availability ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "CONNECTOR",
    "error_description": "Argument 'CONNECTOR' is null or empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method. Application context required | The method was called outside the application context of OAuth ||
|| `400` | `CONNECTOR` | Argument 'CONNECTOR' is null or empty | The connector identifier `CONNECTOR` was not provided ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imconnector-register.md)
- [{#T}](./imconnector-activate.md)
- [{#T}](./imconnector-connector-data-set.md)
- [{#T}](./imconnector-list.md)
- [{#T}](./imconnector-unregister.md)
- [{#T}](./imconnector-send-messages.md)
- [{#T}](./imconnector-update-messages.md)
- [{#T}](./imconnector-delete-messages.md)
- [{#T}](./imconnector-send-status-delivery.md)
- [{#T}](./imconnector-chat-name-set.md)