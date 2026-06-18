# Get Field Settings Description for Custom Field Type crm.userfield.settings.fields

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.userfield.settings.fields` returns the field settings description for the specified custom field type.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **type***
[`string`](../../data-types.md) | The type of the custom field. A value from the list returned by the method [crm.userfield.types](./crm-userfield-types.md) ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"double"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.userfield.settings.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"double","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.userfield.settings.fields
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type UserfieldSettingsItem = {
      type: string
      title: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type UserfieldSettingsResult = Record<string, UserfieldSettingsItem>

    try {
      const response = await $b24.actions.v2.call.make<UserfieldSettingsResult>({
        method: 'crm.userfield.settings.fields',
        params: {
          type: 'double',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(Object.keys(result), result)
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
      async function fetchUserfieldSettingsFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.userfield.settings.fields',
            params: {
              type: 'double',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(Object.keys(result), result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchUserfieldSettingsFields)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.userfield.settings.fields',
                [
                    'type' => 'double'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching CRM userfield settings fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.userfield.settings.fields",
        {
            type: "double"
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.userfield.settings.fields',
        [
            'type' => 'double'
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
        "DEFAULT_VALUE": {
            "type": "double",
            "title": "Default Value"
        },
        "PRECISION": {
            "type": "int",
            "title": "Precision"
        }
    },
    "time": {
        "start": 1752132864.335154,
        "finish": 1752132864.366912,
        "duration": 0.03175783157348633,
        "processing": 0.002053976058959961,
        "date_start": "2025-07-10T10:34:24+02:00",
        "date_finish": "2025-07-10T10:34:24+02:00",
        "operating_reset_at": 1752133464,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response containing field settings. The final list of fields depends on the type of the requested field ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Parameter 'type' is not specified or empty."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty Value | Parameter 'type' is not specified or empty | The parameter `type` is not provided or an empty value is passed ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-userfield-types.md)
- [{#T}](./crm-userfield-fields.md)
- [{#T}](./crm-userfield-enumeration-fields.md)