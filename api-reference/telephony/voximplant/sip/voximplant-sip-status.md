# Get SIP Registration Status voximplant.sip.status

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SipStatusResult = {
      REG_ID: number
      LAST_UPDATED: string
      ERROR_MESSAGE: string
      STATUS_CODE: number
      STATUS_RESULT: 'success' | 'error' | 'in_progress' | 'wait'
    }

    try {
      const response = await $b24.actions.v2.call.make<SipStatusResult>({
        method: 'voximplant.sip.status',
        params: {
          REG_ID: 150907,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.STATUS_RESULT, result.STATUS_CODE, result.ERROR_MESSAGE)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getSipStatus() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'voximplant.sip.status',
            params: {
              REG_ID: 150907,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.STATUS_RESULT, result.STATUS_CODE, result.ERROR_MESSAGE)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getSipStatus)
    </script>
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