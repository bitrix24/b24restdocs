# Unregister the Connector imconnector.unregister

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imconnector.unregister` removes a user connector.

{% note info "" %}

This method works only in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %} 

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`string`](../../data-types.md) | The identifier of the connector that was provided during registration in [imconnector.register](./imconnector-register.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"ID":"myconnector","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imconnector.unregister
    ```

- JS

    ```js
    const response = await $b24.callMethod('imconnector.unregister', {
      ID: 'myconnector',
    });
    console.log(response.getData());
    ```

- PHP

    ```php
    $result = $b24Service->core->call(
        'imconnector.unregister',
        [
            'ID' => 'myconnector',
        ]
    );
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imconnector.unregister',
        {
          ID: 'myconnector',
        },
        function(result) {
          console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    $result = CRest::call(
        'imconnector.unregister',
        [
            'ID' => 'myconnector',
        ]
    );
    ```
{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
    "result": true
    },
    "time": {
    "start": 1738065600.11,
    "finish": 1738065600.25,
    "duration": 0.14,
    "processing": 0.06,
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
[`boolean`](../../data-types.md) | `true` if the connector registration has been removed ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

The method can return two error formats.

1. HTTP Status: **200** with `result = false`

  ```json
    {
        "result": {
            "result": false,
            "error": "APPLICATION_UNREGISTRATION_ERROR",
            "error_description": "Error unregistering the application"
        }
    }
  ```

2. HTTP Status: **403** for a system authorization error

  ```json
    {
        "error": "WRONG_AUTH_TYPE",
        "error_description": "Current authorization type is denied for this method. Application context required."
    }
  ```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `200` | `APPLICATION_UNREGISTRATION_ERROR` | Error unregistering the application | Failed to remove the connector by `ID` for the current application ||
|| `200` | `NO_APPLICATION_ID` | Failed to retrieve application ID | Application not found in the context of the request ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method. Application context required. | Method called outside the context of the OAuth application ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imconnector-register.md)
- [{#T}](./imconnector-activate.md)
- [{#T}](./imconnector-status.md)
- [{#T}](./imconnector-connector-data-set.md)
- [{#T}](./imconnector-list.md)
- [{#T}](./imconnector-send-messages.md)
- [{#T}](./imconnector-update-messages.md)
- [{#T}](./imconnector-delete-messages.md)
- [{#T}](./imconnector-send-status-delivery.md)
- [{#T}](./imconnector-chat-name-set.md)