# Update SIP Line voximplant.sip.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: user with the Manage Numbers — Edit access permission

The method `voximplant.sip.update` updates an existing SIP line created by the current application.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONFIG_ID***
[`integer`](../../../data-types.md) | Identifier of the SIP line configuration.

You can obtain the identifier using the [voximplant.sip.get](./voximplant-sip-get.md) method. ||
|| **TITLE**
[`string`](../../../data-types.md) | New name for the connection. ||
|| **SERVER**
[`string`](../../../data-types.md) | New SIP registration server address. ||
|| **LOGIN**
[`string`](../../../data-types.md) | New login for connecting to the server. ||
|| **PASSWORD**
[`string`](../../../data-types.md) | New password for connecting to the server. Maximum length is 100 characters. ||
|#

{% note info "" %}

To make changes, at least one of the following fields must be provided: `TITLE`, `SERVER`, `LOGIN`, `PASSWORD`.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CONFIG_ID":5,"TITLE":"SIP line 1 (updated)"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/voximplant.sip.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CONFIG_ID":5,"TITLE":"SIP line 1 (updated)","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/voximplant.sip.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'voximplant.sip.update',
        params: {
          CONFIG_ID: 5,
          TITLE: 'SIP line 1 (updated)',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('SIP line updated, result:', result)
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
      async function updateSipLine() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'voximplant.sip.update',
            params: {
              CONFIG_ID: 5,
              TITLE: 'SIP line 1 (updated)',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('SIP line updated, result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateSipLine)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'voximplant.sip.update',
                [
                    'CONFIG_ID' => 5,
                    'TITLE' => 'SIP line 1 (updated)',
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
        'voximplant.sip.update',
        {
            CONFIG_ID: 5,
            TITLE: 'SIP line 1 (updated)'
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
        'voximplant.sip.update',
        [
            'CONFIG_ID' => 5,
            'TITLE' => 'SIP line 1 (updated)',
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
        "start": 1773657589,
        "finish": 1773657589.659046,
        "duration": 0.659045934677124,
        "processing": 0,
        "date_start": "2026-03-16T13:39:49+01:00",
        "date_finish": "2026-03-16T13:39:49+01:00",
        "operating_reset_at": 1773658189,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Result of the update.

`1` — update was successful. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**, **403**, **404**

```json
{
    "error": "ERROR_NOT_FOUND",
    "error_description": "Specified CONFIG_ID is not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_NOT_FOUND` | `Specified CONFIG_ID is not found` | SIP line with the specified `CONFIG_ID` was not found among the lines of the current application. ||
|| `CHECK_FIELDS_ERROR` | `Server address not specified` | An empty or incorrect value was provided for the `SERVER` parameter. ||
|| `CHECK_FIELDS_ERROR` | `Login for connecting to the server not specified` | An empty or incorrect value was provided for the `LOGIN` parameter. ||
|| `CHECK_FIELDS_ERROR` | `Password for connecting to the server cannot exceed 100 characters` | The limit for the `PASSWORD` parameter has been exceeded. ||
|| `TITLE_EXISTS` | `The specified connection name is already registered in the system` | A line with that name already exists. ||
|| `ACCESS_DENIED` | `Access denied!` | Insufficient permissions to update the SIP line. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./voximplant-sip-add.md)
- [{#T}](./voximplant-sip-update.md)
- [{#T}](./voximplant-sip-get.md)
- [{#T}](./voximplant-sip-delete.md)
- [{#T}](./voximplant-sip-status.md)
- [{#T}](./voximplant-sip-connector-status.md)