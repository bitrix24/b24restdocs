# Get SIP Registration Status voximplant.sip.status

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: user with the Manage Numbers — Edit access permission

The method `voximplant.sip.status` returns the current SIP registration status for the cloud PBX.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **REG_ID***
[`integer`](../../../data-types.md) | Identifier of the SIP registration.

The identifier can be obtained using the [voximplant.sip.get](./voximplant-sip-get.md) method. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"REG_ID":150907}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/voximplant.sip.status
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"REG_ID":150907,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/voximplant.sip.status
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'voximplant.sip.status',
            {
                REG_ID: 150907
            }
        );
        
        const result = response.getData().result;
        console.log('Data:', result);
        
        processResult(result);
    }
    catch( error )
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
                'voximplant.sip.status',
                [
                    'REG_ID' => 150907
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'voximplant.sip.status',
        {
            REG_ID: 150907
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
        'voximplant.sip.status',
        [
            'REG_ID' => 150907
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
    "result": {
        "REG_ID": 150907,
        "LAST_UPDATED": "2026-03-12 09:10:51",
        "ERROR_MESSAGE": "",
        "STATUS_CODE": 200,
        "STATUS_RESULT": "success"
    },
    "time": {
        "start": 1773323644,
        "finish": 1773323644.70319,
        "duration": 0.7031900882720947,
        "processing": 0,
        "date_start": "2026-03-12T16:54:04+01:00",
        "date_finish": "2026-03-12T16:54:04+01:00",
        "operating_reset_at": 1773324244,
        "operating": 0.15053892135620117
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object containing the SIP registration status ||
|| **REG_ID**
[`integer`](../../../data-types.md) | Identifier of the SIP registration ||
|| **LAST_UPDATED**
[`string`](../../../data-types.md) | Date and time of the last update of the SIP registration status ||
|| **ERROR_MESSAGE**
[`string`](../../../data-types.md) | Text description of the registration error ||
|| **STATUS_CODE**
[`integer`](../../../data-types.md) | Numeric code of the registration status or error ||
|| **STATUS_RESULT**
[`string`](../../../data-types.md) | Result of the registration.

Possible values:

- `success` — SIP registration completed successfully
- `error` — an error occurred during SIP registration
- `in_progress` — SIP registration is currently in progress
- `wait` — SIP registration is waiting to start ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "REG_ID_NOT_FOUND",
    "error_description": "Settings not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `REG_ID_NOT_FOUND` | `Settings not found` | SIP registration with the specified `REG_ID` was not found ||
|| `ACCESS_DENIED` | `Access denied!` | Insufficient permissions to retrieve the SIP registration status ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./voximplant-sip-add.md)
- [{#T}](./voximplant-sip-connector-status.md)
- [{#T}](./voximplant-sip-delete.md)
- [{#T}](./voximplant-sip-get.md)
- [{#T}](./voximplant-sip-update.md)