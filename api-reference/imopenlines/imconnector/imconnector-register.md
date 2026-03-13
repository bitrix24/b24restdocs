# Register the Connector imconnector.register

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imconnector.register` registers a custom connector for Open Channels.

{% note info "" %}

The method works only in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %} 

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`string`](../../data-types.md) | Unique identifier for the connector. The method converts the value to lowercase. It is recommended to add a unique prefix at the beginning of the identifier to avoid conflicts with existing and future identifiers of other connectors. Use digits, lowercase letters, and the underscore `_` to form the identifier. The `.` character is prohibited.

Use this value in all methods where the connector identifier is required — usually in the `CONNECTOR` parameter ||
|| **NAME***
[`string`](../../data-types.md) | Name of the connector in the interface ||
|| **ICON***
[`object`](../../data-types.md) | Parameters for the main icon. 

The structure of the object is described in detail [below](#icon) ||
|| **PLACEMENT_HANDLER***
[`string`](../../data-types.md) | URL of the embedding handler for the connector settings. At this address, Bitrix24 opens the settings interface in a slider for the user. Read more about embedding interfaces in the article [Widget Embedding Mechanism](../../widgets/index.md) ||
|| **ICON_DISABLED**
[`object`](../../data-types.md) | Parameters for the inactive state icon. 

The structure of the object is described in detail [below](#icon-disabled) ||
|| **DEL_EXTERNAL_MESSAGES**
[`boolean`](../../data-types.md) | Allows deleting incoming messages. Default value is `true` ||
|| **EDIT_INTERNAL_MESSAGES**
[`boolean`](../../data-types.md) | Allows editing operator messages. Default value is `true` ||
|| **DEL_INTERNAL_MESSAGES**
[`boolean`](../../data-types.md) | Allows deleting operator messages. Default value is `true` ||
|| **NEWSLETTER**
[`boolean`](../../data-types.md) | Allows using the channel in CRM newsletters. Default value is `true` ||
|| **NEED_SYSTEM_MESSAGES**
[`boolean`](../../data-types.md) | Allows sending system messages to the channel. Default value is `true` ||
|| **NEED_SIGNATURE**
[`boolean`](../../data-types.md) | Adds the operator's signature to messages. Default value is `true` ||
|| **CHAT_GROUP**
[`boolean`](../../data-types.md) | Indicates the chat processing mode of the connector: `true` — grouping by `chat.id` (group chat), `false` — by `user.id` (one-on-one chat). Default value is `false` ||
|| **COMMENT**
[`string`](../../data-types.md) | Text explanation displayed in the connector settings block in the slider ||
|#

### ICON Parameter {#icon}

#|
|| **Name**
`type` | **Description** ||
|| **DATA_IMAGE***
[`string`](../../data-types.md) | SVG icon in Data URI format: a string with the prefix `data:image/svg+xml,`, followed by the SVG content, usually URL-encoded ||
|| **COLOR**
[`string`](../../data-types.md) | Background color of the icon. Example: `#69acc0` ||
|| **SIZE**
[`string`](../../data-types.md) | Background size. Example: `90%` ||
|| **POSITION**
[`string`](../../data-types.md) | Background position. Example: `center` ||
|#

### ICON_DISABLED Parameter {#icon-disabled}

#|
|| **Name**
`type` | **Description** ||
|| **DATA_IMAGE**
[`string`](../../data-types.md) | SVG icon for the inactive state in Data URI format: a string with the prefix `data:image/svg+xml,`, followed by the SVG content, usually URL-encoded ||
|| **COLOR**
[`string`](../../data-types.md) | Background color of the inactive icon ||
|| **SIZE**
[`string`](../../data-types.md) | Background size of the inactive icon ||
|| **POSITION**
[`string`](../../data-types.md) | Background position of the inactive icon ||
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
        "ID": "myconnector",
        "NAME": "My Connector",
        "ICON": {
          "DATA_IMAGE": "data:image/svg+xml,%3Csvg%20xmlns%3D%22http://www.w3.org/2000/svg%22/%3E",
          "COLOR": "#69acc0",
          "SIZE": "90%",
          "POSITION": "center"
        },
        "PLACEMENT_HANDLER": "https://example.com/connector/settings",
        "ICON_DISABLED": {
          "DATA_IMAGE": "data:image/svg+xml,%3Csvg%20xmlns%3D%22http://www.w3.org/2000/svg%22/%3E",
          "COLOR": "#99adb3"
        },
        "DEL_EXTERNAL_MESSAGES": true,
        "EDIT_INTERNAL_MESSAGES": true,
        "DEL_INTERNAL_MESSAGES": true,
        "NEWSLETTER": true,
        "NEED_SYSTEM_MESSAGES": true,
        "NEED_SIGNATURE": true,
        "CHAT_GROUP": false,
        "COMMENT": "Channel settings",
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/imconnector.register
    ```

- JS

    ```js
    const payload = {
      ID: 'myconnector',
      NAME: 'My Connector',
      ICON: {
        DATA_IMAGE: 'data:image/svg+xml,%3Csvg%20xmlns%3D%22http://www.w3.org/2000/svg%22/%3E',
        COLOR: '#69acc0',
        SIZE: '90%',
        POSITION: 'center',
      },
      PLACEMENT_HANDLER: 'https://example.com/connector/settings',
      ICON_DISABLED: {
        DATA_IMAGE: 'data:image/svg+xml,%3Csvg%20xmlns%3D%22http://www.w3.org/2000/svg%22/%3E',
        COLOR: '#99adb3',
      },
      DEL_EXTERNAL_MESSAGES: true,
      EDIT_INTERNAL_MESSAGES: true,
      DEL_INTERNAL_MESSAGES: true,
      NEWSLETTER: true,
      NEED_SYSTEM_MESSAGES: true,
      NEED_SIGNATURE: true,
      CHAT_GROUP: false,
      COMMENT: 'Channel settings',
    };

    const response = await $b24.callMethod('imconnector.register', payload);
    console.log(response.getData());
    ```

- PHP

    ```php
    $result = $b24Service->core->call(
        'imconnector.register',
        [
            'ID' => 'myconnector',
            'NAME' => 'My Connector',
            'ICON' => [
                'DATA_IMAGE' => 'data:image/svg+xml,%3Csvg%20xmlns%3D%22http://www.w3.org/2000/svg%22/%3E',
                'COLOR' => '#69acc0',
                'SIZE' => '90%',
                'POSITION' => 'center',
            ],
            'PLACEMENT_HANDLER' => 'https://example.com/connector/settings',
            'ICON_DISABLED' => [
                'DATA_IMAGE' => 'data:image/svg+xml,%3Csvg%20xmlns%3D%22http://www.w3.org/2000/svg%22/%3E',
                'COLOR' => '#99adb3',
            ],
            'DEL_EXTERNAL_MESSAGES' => true,
            'EDIT_INTERNAL_MESSAGES' => true,
            'DEL_INTERNAL_MESSAGES' => true,
            'NEWSLETTER' => true,
            'NEED_SYSTEM_MESSAGES' => true,
            'NEED_SIGNATURE' => true,
            'CHAT_GROUP' => false,
            'COMMENT' => 'Channel settings',
        ]
    );
    ```

- BX24.js

    ```js
    BX24.callMethod(
      'imconnector.register',
      {
        ID: 'myconnector',
        NAME: 'My Connector',
        ICON: {
          DATA_IMAGE: 'data:image/svg+xml,%3Csvg%20xmlns%3D%22http://www.w3.org/2000/svg%22/%3E',
          COLOR: '#69acc0',
          SIZE: '90%',
          POSITION: 'center',
        },
        PLACEMENT_HANDLER: 'https://example.com/connector/settings',
        ICON_DISABLED: {
          DATA_IMAGE: 'data:image/svg+xml,%3Csvg%20xmlns%3D%22http://www.w3.org/2000/svg%22/%3E',
          COLOR: '#99adb3',
        },
        DEL_EXTERNAL_MESSAGES: true,
        EDIT_INTERNAL_MESSAGES: true,
        DEL_INTERNAL_MESSAGES: true,
        NEWSLETTER: true,
        NEED_SYSTEM_MESSAGES: true,
        NEED_SIGNATURE: true,
        CHAT_GROUP: false,
        COMMENT: 'Channel settings',
      },
      function(result) {
        console.log(result.data());
      }
    );
    ```

- PHP CRest

    ```php
    $result = CRest::call(
        'imconnector.register',
        [
            'ID' => 'myconnector',
            'NAME' => 'My Connector',
            'ICON' => [
                'DATA_IMAGE' => 'data:image/svg+xml,%3Csvg%20xmlns%3D%22http://www.w3.org/2000/svg%22/%3E',
                'COLOR' => '#69acc0',
                'SIZE' => '90%',
                'POSITION' => 'center',
            ],
            'PLACEMENT_HANDLER' => 'https://example.com/connector/settings',
            'ICON_DISABLED' => [
                'DATA_IMAGE' => 'data:image/svg+xml,%3Csvg%20xmlns%3D%22http://www.w3.org/2000/svg%22/%3E',
                'COLOR' => '#99adb3',
            ],
            'DEL_EXTERNAL_MESSAGES' => true,
            'EDIT_INTERNAL_MESSAGES' => true,
            'DEL_INTERNAL_MESSAGES' => true,
            'NEWSLETTER' => true,
            'NEED_SYSTEM_MESSAGES' => true,
            'NEED_SIGNATURE' => true,
            'CHAT_GROUP' => false,
            'COMMENT' => 'Channel settings',
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
    "finish": 1738065600.38,
    "duration": 0.27,
    "processing": 0.10,
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
[`boolean`](../../data-types.md) | `true` if the connector was successfully registered ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

The method can return two error formats:

1. HTTP Status: **200** with `result = false`

  ```json
  {
      "result": {
          "result": false,
          "error": "CONNECTOR_ID_REQUIRED",
          "error_description": "Connector ID is not specified"
      }
  }
  ```

2. HTTP Status: **403** for a system authorization error

  ```json
  {
      "error": "WRONG_AUTH_TYPE",
      "error_description": "Current authorization type is denied for this method Application context required"
  }
  ```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `200` | `APPLICATION_REGISTRATION_ERROR_POINT` | Application registration error. The connector identifier cannot contain a dot | The `ID` contains the `.` character ||
|| `200` | `CONNECTOR_ID_REQUIRED` | Connector ID is not specified | Empty or non-string `ID` ||
|| `200` | `NAME_REQUIRED` | Connector name is not specified | Empty or non-string `NAME` ||
|| `200` | `ICON_REQUIRED` | Connector icon is not specified | `ICON.DATA_IMAGE` is not provided or not a string ||
|| `200` | `NO_APPLICATION_ID` | Failed to get application ID | No application found in the request context ||
|| `200` | `NO_PLACEMENT_HANDLER` | Failed to get the embedding handler URL | Empty or non-string `PLACEMENT_HANDLER` ||
|| `200` | `APPLICATION_REGISTRATION_ERROR` | Application registration error | Failed to complete registration: connector data and embedding parameters were not saved ||
|| `200` | `GENERAL_CONNECTOR_REGISTRATION_ERROR` | General connector registration error | Other validation errors in input data ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method Application context required | Method called outside of the OAuth application context ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imconnector-activate.md)
- [{#T}](./imconnector-status.md)
- [{#T}](./imconnector-connector-data-set.md)
- [{#T}](./imconnector-list.md)
- [{#T}](./imconnector-unregister.md)
- [{#T}](./imconnector-send-messages.md)
- [{#T}](./imconnector-update-messages.md)
- [{#T}](./imconnector-delete-messages.md)
- [{#T}](./imconnector-send-status-delivery.md)
- [{#T}](./imconnector-chat-name-set.md)
- [{#T}](../../../tutorials/openlines/example-connector.md)