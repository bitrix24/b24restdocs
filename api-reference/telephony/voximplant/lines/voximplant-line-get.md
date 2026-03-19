# Get a List of Outgoing Lines voximplant.line.get

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the Management of Numbers — modification access permission

The method `voximplant.line.get` returns a list of available outgoing lines.

## Method Parameters

No parameters.

## Code Examples

{% include [Example Footnote](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/voximplant.line.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/voximplant.line.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'voximplant.line.get',
            {}
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
                'voximplant.line.get',
                []
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
        'voximplant.line.get',
        {},
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
        'voximplant.line.get',
        []
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
    "result": {
        "reg150907": "SIP line",
        "sip7": "Office PBX 1",
        "reg151083": "Cloud PBX (9)",
        "sip11": "Office PBX (11)",
        "reg151085": "SIP line 2"
    },
    "time": {
        "start": 1773664148,
        "finish": 1773664148.538045,
        "duration": 0.5380449295043945,
        "processing": 0,
        "date_start": "2026-03-16T15:29:08+02:00",
        "date_finish": "2026-03-16T15:29:08+02:00",
        "operating_reset_at": 1773664748,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | An object of the form `LINE_ID: LINE_NAME`, where:

- `LINE_ID` — line identifier
- `LINE_NAME` — line name ||
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
|| `ACCESS_DENIED` | `Access denied!` | Insufficient permissions to retrieve the list of outgoing lines ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./voximplant-line-get.md)
- [{#T}](./voximplant-line-outgoing-get.md)
- [{#T}](./voximplant-line-outgoing-set.md)
- [{#T}](./voximplant-line-outgoing-sip-set.md)