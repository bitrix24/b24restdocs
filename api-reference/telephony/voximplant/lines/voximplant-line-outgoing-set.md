# Set Outgoing Line voximplant.line.outgoing.set

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: user with the Management of Numbers — modification access permission

The method `voximplant.line.outgoing.set` sets the default outgoing line.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **LINE_ID**
[`string`](../../../data-types.md) | Identifier of the line.

Available identifiers can be obtained using the [voximplant.line.get](./voximplant-line-get.md) method ||
|#

{% note info "" %}

The change occurs only if `LINE_ID` exists among the available lines. If `LINE_ID` does not exist or is not specified, the method will return `1`, as it would upon a successful setting, but the configuration will not change.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"LINE_ID":"reg150907"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/voximplant.line.outgoing.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"LINE_ID":"reg150907","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/voximplant.line.outgoing.set
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'voximplant.line.outgoing.set',
            {
                LINE_ID: 'reg150907'
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
                'voximplant.line.outgoing.set',
                [
                    'LINE_ID' => 'reg150907',
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
        'voximplant.line.outgoing.set',
        {
            LINE_ID: 'reg150907'
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
        'voximplant.line.outgoing.set',
        [
            'LINE_ID' => 'reg150907',
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
        "start": 1773664560,
        "finish": 1773664560.138283,
        "duration": 0.13828301429748535,
        "processing": 0,
        "date_start": "2026-03-16T15:36:00+02:00",
        "date_finish": "2026-03-16T15:36:00+02:00",
        "operating_reset_at": 1773665160,
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

`1` — line set ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **403**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied!"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ACCESS_DENIED` | `Access denied!` | Insufficient rights to modify the default outgoing line ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./voximplant-line-get.md)
- [{#T}](./voximplant-line-outgoing-get.md)
- [{#T}](./voximplant-line-outgoing-set.md)
- [{#T}](./voximplant-line-outgoing-sip-set.md)