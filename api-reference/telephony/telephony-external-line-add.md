# Add External Line telephony.externalLine.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`telephony`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `telephony.externalLine.add` adds an external line to the application.

{% note info "" %}

The method works only in the context of the [application](../../settings/app-installation/index.md).

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NUMBER***
[`string`](../data-types.md) | External line number ||
|| **NAME**
[`string`](../data-types.md) | Name of the external line ||
|| **CRM_AUTO_CREATE**
[`string`](../data-types.md) | Auto-creation of a CRM object for outgoing calls.

Possible values:

 `Y` — enabled
 `N` — disabled
 
Default — `Y` ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"NUMBER":"14151234567","NAME":"Main External Line","CRM_AUTO_CREATE":"Y","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/telephony.externalLine.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ExternalLineAddResult = {
      ID: number
    }

    try {
      const response = await $b24.actions.v2.call.make<ExternalLineAddResult>({
        method: 'telephony.externalLine.add',
        params: {
          NUMBER: '74951234567',
          NAME: 'Main external line',
          CRM_AUTO_CREATE: 'Y',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Added external line with ID:', result.ID)
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
      async function addExternalLine() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'telephony.externalLine.add',
            params: {
              NUMBER: '74951234567',
              NAME: 'Main external line',
              CRM_AUTO_CREATE: 'Y',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Added external line with ID:', result.ID)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addExternalLine)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'telephony.externalLine.add',
                [
                    'NUMBER' => '14151234567',
                    'NAME' => 'Main External Line',
                    'CRM_AUTO_CREATE' => 'Y'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding external line: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "telephony.externalLine.add",
        {
            NUMBER: '14151234567',
            NAME: 'Main External Line',
            CRM_AUTO_CREATE: 'Y'
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
        'telephony.externalLine.add',
        [
            'NUMBER' => '14151234567',
            'NAME' => 'Main External Line',
            'CRM_AUTO_CREATE' => 'Y'
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
        "ID": 7
    },
    "time": {
        "start": 1772801648,
        "finish": 1772801648.420551,
        "duration": 0.420551061630249,
        "processing": 0,
        "date_start": "2026-03-06T15:54:08+01:00",
        "date_finish": "2026-03-06T15:54:08+01:00",
        "operating_reset_at": 1772802248,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root element of the response ||
|| **ID**
[`integer`](../data-types.md) | Identifier of the created external line ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Current authorization type is denied for this method"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `WRONG_AUTH_TYPE` | Current authorization type is denied for this method | Method called outside the context of the application ||
|| `ERROR_CORE` | NUMBER should not be empty | Required parameter `NUMBER` not provided ||
|| `ERROR_CORE` | Line already exists | A line with the specified number already exists ||
|| `ERROR_CORE` | DB error | Database error while adding the line ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./telephony-external-line-update.md)
- [{#T}](./telephony-external-line-get.md)
- [{#T}](./telephony-external-line-delete.md)
- [{#T}](./telephony-external-call-register.md)