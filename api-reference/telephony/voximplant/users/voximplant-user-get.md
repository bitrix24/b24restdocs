# Get User Settings voximplant.user.get

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: user with User Settings — Modify access permission

The method `voximplant.user.get` returns user settings.

For applications, calling the method requires [administrator confirmation](../../../scopes/confirmation.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **USER_ID***
[`integer/array`](../../../data-types.md) | User ID or an array of user IDs.

You can obtain the ID using the [user.get](../../../user/user-get.md) method. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":[1269, 1271]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/voximplant.user.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":[1269, 1271],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/voximplant.user.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'voximplant.user.get',
            {
                USER_ID: [1269, 1271]
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
                'voximplant.user.get',
                [
                    'USER_ID' => [1269, 1271],
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
        'voximplant.user.get',
        {
            USER_ID: [1269, 1271]
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
        'voximplant.user.get',
        [
            'USER_ID' => [1269, 1271],
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
    "result": [
        {
        "ID": "1269",
        "DEFAULT_LINE": null,
        "PHONE_ENABLED": "Y",
        "SIP_SERVER": "ip.b24-6059-1584795982.bitrixphone.com",
        "SIP_LOGIN": "phone1269",
        "SIP_PASSWORD": "28fec473a88a",
        "INNER_NUMBER": null
        },
        {
        "ID": "1271",
        "DEFAULT_LINE": null,
        "PHONE_ENABLED": "Y",
        "SIP_SERVER": "ip.b24-6059-158475982.bitrixphone.com",
        "SIP_LOGIN": "phone1271",
        "SIP_PASSWORD": null,
        "INNER_NUMBER": null
        }
    ],
    "time": {
        "start": 1773667386,
        "finish": 1773667386.23801,
        "duration": 0.23800992965698242,
        "processing": 0,
        "date_start": "2026-03-16T16:23:06+01:00",
        "date_finish": "2026-03-16T16:23:06+01:00",
        "operating_reset_at": 1773667986,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Array of user settings ||
|| **ID**
[`string`](../../../data-types.md) | User ID ||
|| **DEFAULT_LINE**
[`string`](../../../data-types.md) | Default outgoing line number ||
|| **PHONE_ENABLED**
[`string`](../../../data-types.md) | Indicator of SIP device availability.

Possible values:
- `Y` — SIP device is activated for the user
- `N` — SIP device is not activated for the user ||
|| **SIP_SERVER**
[`string`](../../../data-types.md) | Address of the server for connecting the SIP device ||
|| **SIP_LOGIN**
[`string`](../../../data-types.md) | Login for connecting the SIP device ||
|| **SIP_PASSWORD**
[`string`](../../../data-types.md) | Password for connecting the SIP device ||
|| **INNER_NUMBER**
[`string`](../../../data-types.md) | User's internal number ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **401**, **403**

```json
{
    "error": "METHOD_CONFIRM_WAITING",
    "error_description": "Waiting for confirmation"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| — | `Parameter USER_ID is not set` | Required parameter `USER_ID` is not specified ||
|| `METHOD_CONFIRM_WAITING` | `Waiting for confirmation` | Administrator confirmation is pending for the method call ||
|| `METHOD_CONFIRM_DENIED` | `Method call denied` | Administrator has denied the method call for the current token ||
|| `ACCESS_DENIED` | `Access denied! You have no permission to query selected users` | Insufficient permissions to retrieve user settings ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./voximplant-user-get.md)
- [{#T}](./voximplant-user-activate-phone.md)
- [{#T}](../../../scopes/confirmation.md)