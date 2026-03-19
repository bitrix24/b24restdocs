# Set Outgoing SIP Line voximplant.line.outgoing.sip.set

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: user with the Management of Numbers — modification access permission

The method `voximplant.line.outgoing.sip.set` sets the SIP line as the default outgoing line.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONFIG_ID***
[`integer`](../../../data-types.md) | Identifier of the SIP line configuration.

You can obtain the identifier using the [voximplant.sip.get](../sip/voximplant-sip-get.md) method. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CONFIG_ID":9}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/voximplant.line.outgoing.sip.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CONFIG_ID":9,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/voximplant.line.outgoing.sip.set
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'voximplant.line.outgoing.sip.set',
            {
                CONFIG_ID: 9
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
                'voximplant.line.outgoing.sip.set',
                [
                    'CONFIG_ID' => 9,
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
        'voximplant.line.outgoing.sip.set',
        {
            CONFIG_ID: 9
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
        'voximplant.line.outgoing.sip.set',
        [
            'CONFIG_ID' => 9,
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
        "start": 1773665506,
        "finish": 1773665506.748929,
        "duration": 0.7489290237426758,
        "processing": 0,
        "date_start": "2026-03-16T15:51:46+01:00",
        "date_finish": "2026-03-16T15:51:46+01:00",
        "operating_reset_at": 1773666106,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Result of the method execution.

`1` — SIP line has been set. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "Specified CONFIG_ID is not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | `Specified CONFIG_ID is not found` | SIP line with the specified `CONFIG_ID` was not found. ||
|| `ACCESS_DENIED` | `Access denied!` | Insufficient rights to modify the outgoing SIP line. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./voximplant-line-get.md)
- [{#T}](./voximplant-line-outgoing-get.md)
- [{#T}](./voximplant-line-outgoing-set.md)
- [{#T}](./voximplant-line-outgoing-sip-set.md)