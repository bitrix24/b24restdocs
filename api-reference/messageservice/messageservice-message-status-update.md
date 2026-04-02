# Update Message Delivery Status messageservice.message.status.update

> Scope: [`messageservice`](../scopes/permissions.md)
>
> Who can execute the method: message sender or administrator

The method `messageservice.message.status.update` updates the delivery status of a message sent through a messaging provider.

This method works only within the context of an [application](../../settings/app-installation/index.md).

## Method Parameters

{% include [Footnote on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CODE***
[`string`](../data-types.md) | Provider code.

The provider code can be obtained using the [messageservice.sender.list](./messageservice-sender-list.md) method. ||
|| **MESSAGE_ID***
[`string`](../data-types.md) | External identifier of the message, received by the application handler when the message was sent. ||
|| **STATUS***
[`string`](../data-types.md) | New status of the message.

Supported values:
- `queued` — message is queued for sending
- `sent` — message has been sent by the provider
- `delivered` — message has been successfully delivered to the recipient
- `undelivered` — message was not delivered to the recipient
- `failed` — error occurred while sending or processing the message by the provider ||
|#

## Code Examples

{% include [Footnote on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"provider1","MESSAGE_ID":"65575980fa531ac284c2ee68f81ebebd","STATUS":"delivered","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/messageservice.message.status.update
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'messageservice.message.status.update',
            {
                CODE: 'provider1',
                MESSAGE_ID: '65575980fa531ac284c2ee68f81ebebd',
                STATUS: 'delivered'
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
                'messageservice.message.status.update',
                [
                    'CODE' => 'provider1',
                    'MESSAGE_ID' => '65575980fa531ac284c2ee68f81ebebd',
                    'STATUS' => 'delivered',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating message status: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'messageservice.message.status.update',
        {
            CODE: 'provider1',
            MESSAGE_ID: '65575980fa531ac284c2ee68f81ebebd',
            STATUS: 'delivered'
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
        'messageservice.message.status.update',
        [
            'CODE' => 'provider1',
            'MESSAGE_ID' => '65575980fa531ac284c2ee68f81ebebd',
            'STATUS' => 'delivered',
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
[`boolean`](../data-types.md) | `true` if the message status was successfully updated. ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_MESSAGE_STATUS_INCORRECT",
    "error_description": "Message status incorrect!"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_SENDER_VALIDATION_FAILURE` | `Empty sender code!` | Required parameter `CODE` is missing. ||
|| `ERROR_SENDER_VALIDATION_FAILURE` | `Wrong sender code!` | `CODE` contains invalid characters. ||
|| `ERROR_MESSAGE_NOT_FOUND` | `Message not found!` | Message with the specified `MESSAGE_ID` was not found. ||
|| `ERROR_MESSAGE_STATUS_INCORRECT` | `Message status incorrect!` | An unsupported `STATUS` was provided. ||
|| `ACCESS_DENIED` | `Application context required` | Method was called outside the application context. ||
|| `ACCESS_DENIED` | `Access denied!` | Method was invoked by a non-administrator. ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./messageservice-sender-add.md)
- [{#T}](./messageservice-sender-update.md)
- [{#T}](./messageservice-sender-list.md)
- [{#T}](./messageservice-sender-delete.md)
- [{#T}](./messageservice-message-status-update.md)