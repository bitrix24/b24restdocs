# Update Message Provider messageservice.sender.update

> Scope: [`messageservice`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `messageservice.sender.update` updates the data of a registered message provider.

This method works only in the context of an [application](../../settings/app-installation/index.md).

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CODE***
[`string`](../data-types.md) | The code of the provider to be updated.

The provider code can be obtained using the [messageservice.sender.list](./messageservice-sender-list.md) method. ||
|| **HANDLER**
[`string`](../data-types.md) | New application handler URL. ||
|| **NAME**
[`string` \| `object`](../data-types.md) | New name of the provider.

It can be a string or an associative array of localized strings in the following format:

```js
'NAME': {
    'de': 'Anbietername',
    'en': 'provider name',
    ...
},
```
 ||
|| **DESCRIPTION**
[`string` \| `object`](../data-types.md) | New description of the provider.

It can be a string or an associative array of localized strings in the following format:

```js
'DESCRIPTION': {
    'de': 'Anbieterbeschreibung',
    'en': 'provider description',
    ...
},
```
||
|#

{% note info "" %}

The request must include `CODE` and at least one of the parameters `HANDLER`, `NAME`, or `DESCRIPTION`.

{% endnote %}

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"provider1","HANDLER":"https://provider.example/api/new-handler","NAME":"Provider 1 Updated","DESCRIPTION":"Updated description","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/messageservice.sender.update
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'messageservice.sender.update',
            {
                CODE: 'provider1',
                HANDLER: 'https://provider.example/api/new-handler',
                NAME: 'Provider 1 Updated',
                DESCRIPTION: 'Updated description'
            }
        );

        const result = response.getData().result;
        console.log(result);
    }
    catch (error)
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'messageservice.sender.update',
                [
                    'CODE' => 'provider1',
                    'HANDLER' => 'https://provider.example/api/new-handler',
                    'NAME' => 'Provider 1 Updated',
                    'DESCRIPTION' => 'Updated description',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating sender: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'messageservice.sender.update',
        {
            CODE: 'provider1',
            HANDLER: 'https://provider.example/api/new-handler',
            NAME: 'Provider 1 Updated',
            DESCRIPTION: 'Updated description'
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'messageservice.sender.update',
        [
            'CODE' => 'provider1',
            'HANDLER' => 'https://provider.example/api/new-handler',
            'NAME' => 'Provider 1 Updated',
            'DESCRIPTION' => 'Updated description',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1742895600,
        "finish": 1742895600.845505,
        "duration": 0.845505952835083,
        "processing": 0.1402289867401123,
        "date_start": "2025-03-25T10:00:00+01:00",
        "date_finish": "2025-03-25T10:00:00+01:00",
        "operating_reset_at": 1742896200,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | `true` if the provider was successfully updated. ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_SENDER_OTHER_PARAMS_REQUIRED",
    "error_description": "At least one other parameter is required!"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_SENDER_CODE_REQUIRED` | `CODE is required!` | The required parameter `CODE` is missing. ||
|| `ERROR_SENDER_OTHER_PARAMS_REQUIRED` | `At least one other parameter is required!` | No parameters for updating (`HANDLER`, `NAME`, `DESCRIPTION`) were provided. ||
|| `ERROR_SENDER_NOT_FOUND` | `Sender not found!` | The provider with the given `CODE` was not found. ||
|| `ERROR_SENDER_UPDATE_FAILURE` | `Sender update error!` | An error occurred while updating the provider. ||
|| `ACCESS_DENIED` | `Application context required` | The method was called outside the application context. ||
|| `ACCESS_DENIED` | `Access denied!` | The method was executed by a non-administrator. ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./messageservice-sender-add.md)
- [{#T}](./messageservice-sender-update.md)
- [{#T}](./messageservice-sender-list.md)
- [{#T}](./messageservice-sender-delete.md)
- [{#T}](./messageservice-message-status-update.md)