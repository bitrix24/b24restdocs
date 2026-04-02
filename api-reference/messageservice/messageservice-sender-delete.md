# Delete SMS Provider or Message Provider messageservice.sender.delete

> Scope: [`messageservice`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `messageservice.sender.delete` removes a previously registered message provider for the current application.

This method works only within the context of an [application](../../settings/app-installation/index.md).

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CODE***
[`string`](../data-types.md) | Provider code.

The provider code can be obtained using the [messageservice.sender.list](./messageservice-sender-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"provider1","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/messageservice.sender.delete
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'messageservice.sender.delete',
            {
                CODE: 'provider1'
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
                'messageservice.sender.delete',
                [
                    'CODE' => 'provider1'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting sender: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'messageservice.sender.delete',
        {
            CODE: 'provider1'
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
        'messageservice.sender.delete',
        [
            'CODE' => 'provider1'
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
        "processing": 0,
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
[`boolean`](../data-types.md) | `true` if the provider was deleted ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_SENDER_NOT_FOUND",
    "error_description": "Sender not found!"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_SENDER_VALIDATION_FAILURE` | `Empty sender code!` | Required parameter `CODE` not provided ||
|| `ERROR_SENDER_VALIDATION_FAILURE` | `Wrong sender code!` | `CODE` contains invalid characters ||
|| `ERROR_SENDER_NOT_FOUND` | `Sender not found!` | Provider with the given `CODE` not found ||
|| `ACCESS_DENIED` | `Application context required` | Method called outside of application context ||
|| `ACCESS_DENIED` | `Access denied!` | Method was not executed by an administrator ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./messageservice-sender-add.md)
- [{#T}](./messageservice-sender-update.md)
- [{#T}](./messageservice-sender-list.md)
- [{#T}](./messageservice-sender-delete.md)
- [{#T}](./messageservice-message-status-update.md)