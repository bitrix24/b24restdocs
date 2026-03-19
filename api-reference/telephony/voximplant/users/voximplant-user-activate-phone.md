# Activate User SIP Device voximplant.user.activatePhone

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: user with User Settings — Modify access permission

The method `voximplant.user.activatePhone` sets a flag indicating the presence of a SIP device for an employee.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **USER_ID***
[`integer`](../../../data-types.md) | User identifier.

You can obtain the identifier using the [user.get](../../../user/user-get.md) method.
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":1269}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/voximplant.user.activatePhone
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":1269,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/voximplant.user.activatePhone
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'voximplant.user.activatePhone',
            {
                USER_ID: 1269
            }
        );

        const result = response.getData().result;
        console.log('Data:', result);
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
                'voximplant.user.activatePhone',
                [
                    'USER_ID' => 1269,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'voximplant.user.activatePhone',
        {
            USER_ID: 1269
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
        'voximplant.user.activatePhone',
        [
            'USER_ID' => 1269,
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
    "result": 1,
    "time": {
        "start": 1773666334,
        "finish": 1773666335.198412,
        "duration": 1.1984119415283203,
        "processing": 1,
        "date_start": "2026-03-16T16:05:34+01:00",
        "date_finish": "2026-03-16T16:05:35+01:00",
        "operating_reset_at": 1773666934,
        "operating": 0.4791221618652344
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Result of the method execution.

`1` — User SIP device activated ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Parameter USER_ID is not set"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| — | `Parameter USER_ID is not set` | Required parameter `USER_ID` is not specified ||
|| — | `You are not allowed to modify user's settings` | Insufficient permissions to activate the user's SIP device ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./voximplant-user-get.md)
- [{#T}](./voximplant-user-activate-phone.md)