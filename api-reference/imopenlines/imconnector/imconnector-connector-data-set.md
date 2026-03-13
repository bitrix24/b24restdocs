# Set Connector Settings imconnector.connector.data.set

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imconnector.connector.data.set` sets the channel settings in an external system for a custom connector and the specified open line.

{% note info "" %}

The method works only in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %} 

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONNECTOR***
[`string`](../../data-types.md) | String code of the connector specified in the `ID` parameter when calling [imconnector.register](./imconnector-register.md) ||
|| **LINE***
[`integer`](../../data-types.md) | Identifier of the open line. 

The identifier can be obtained using the methods [imopenlines.config.get](../openlines/imopenlines-config-get.md) and [imopenlines.config.list.get](../openlines/imopenlines-config-list-get.md) ||
|| **DATA***
[`object`](../../data-types.md) | Object containing the channel settings in the external system. The method does not support setting or modifying a single field separately: settings are written and overwritten through this object as a whole. 

The structure of the object is described in detail [below](#data) ||
|#

### DATA Parameter {#data}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Identifier of the channel in the external system, for example `channel-123`. 

The identifier should be taken from the external system. If none is available, use a unique key, such as a combination of the connector ID and line: `myconnector_line107` ||
|| **URL**
[`string`](../../data-types.md) | Full link to the chat or channel in the external system ||
|| **URL_IM**
[`string`](../../data-types.md) | Full link to the chat in the operator interface format. If there is no separate link for the operator, use the value of `URL` ||
|| **NAME**
[`string`](../../data-types.md) | Name of the channel for display in the interface ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "CONNECTOR":"myconnector",
        "LINE":107,
        "DATA":{
          "ID":"channel-123",
          "URL":"https://example.com/chats/123",
          "NAME":"Support Channel"
        },
        "auth":"**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/imconnector.connector.data.set
    ```

- JS

    ```js
    const response = await $b24.callMethod('imconnector.connector.data.set', {
      CONNECTOR: 'myconnector',
      LINE: 107,
      DATA: {
        ID: 'channel-123',
        URL: 'https://example.com/chats/123',
        NAME: 'Support Channel',
      },
    });
    console.log(response.getData());
    ```

- PHP

    ```php
    $result = $b24Service->core->call(
        'imconnector.connector.data.set',
        [
            'CONNECTOR' => 'myconnector',
            'LINE' => 107,
            'DATA' => [
                'ID' => 'channel-123',
                'URL' => 'https://example.com/chats/123',
                'NAME' => 'Support Channel',
            ],
        ]
    );
    ```

- BX24.js

    ```js
    BX24.callMethod(
      'imconnector.connector.data.set',
      {
        CONNECTOR: 'myconnector',
        LINE: 107,
        DATA: {
          ID: 'channel-123',
          URL: 'https://example.com/chats/123',
          NAME: 'Support Channel',
        },
      },
      function(result) {
        console.log(result.data());
      }
    );
    ```

- PHP CRest

    ```php
    $result = CRest::call(
        'imconnector.connector.data.set',
        [
            'CONNECTOR' => 'myconnector',
            'LINE' => 107,
            'DATA' => [
                'ID' => 'channel-123',
                'URL' => 'https://example.com/chats/123',
                'NAME' => 'Support Channel',
            ],
        ]
    );
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
    "start": 1738065600.11,
    "finish": 1738065600.20,
    "duration": 0.09,
    "processing": 0.04,
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
[`boolean`](../../data-types.md) | `true` if the data is saved ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "Argument 'DATA' is null or empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method Application context required | Method called outside the application context OAuth ||
|| `400` | `ERROR_ARGUMENT` | Argument 'CONNECTOR' is null or empty | `CONNECTOR` not provided ||
|| `400` | `ERROR_ARGUMENT` | Argument 'LINE' is null or empty | `LINE` not provided ||
|| `400` | `ERROR_ARGUMENT` | Argument 'DATA' is null or empty | `DATA` not provided ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imconnector-register.md)
- [{#T}](./imconnector-activate.md)
- [{#T}](./imconnector-status.md)
- [{#T}](./imconnector-list.md)
- [{#T}](./imconnector-unregister.md)
- [{#T}](./imconnector-send-messages.md)
- [{#T}](./imconnector-update-messages.md)
- [{#T}](./imconnector-delete-messages.md)
- [{#T}](./imconnector-send-status-delivery.md)
- [{#T}](./imconnector-chat-name-set.md)
- [{#T}](../../../tutorials/openlines/example-connector.md)