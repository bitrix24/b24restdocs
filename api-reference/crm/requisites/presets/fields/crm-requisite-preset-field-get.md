# Get Custom Field of Requisite Template by ID crm.requisite.preset.field.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns the description of the custom field of the requisite template by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../../data-types.md) | Identifier of the custom field.

Identifiers of custom fields of the requisite template can be obtained using the [crm.requisite.preset.field.list](./crm-requisite-preset-field-list.md) method ||
|| **preset***
[`object`](../../../../data-types.md) | An object containing the identifier of the template from which the information about the custom field is extracted (for example, `{"ID": 27}`).

Template identifiers can be obtained using the [crm.requisite.preset.list](../crm-requisite-preset-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":1,"preset":{"ID":27}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.preset.field.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":1,"preset":{"ID":27},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.preset.field.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type PresetFieldResult = {
      ID: number
      FIELD_NAME: string
      FIELD_TITLE: string
      IN_SHORT_LIST: string
      SORT: number
    }

    try {
      const response = await $b24.actions.v2.call.make<PresetFieldResult>({
        method: 'crm.requisite.preset.field.get',
        params: {
          ID: 1,
          preset: {
            ID: 27,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.ID, result.FIELD_NAME, result.FIELD_TITLE, result.SORT)
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
      async function getPresetField() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.requisite.preset.field.get',
            params: {
              ID: 1,
              preset: {
                ID: 27,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.ID, result.FIELD_NAME, result.FIELD_TITLE, result.SORT)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getPresetField)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.requisite.preset.field.get',
                [
                    'ID'     => 1,
                    'preset' => [
                        'ID' => 27,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting preset field: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.requisite.preset.field.get",
        {
            ID: 1,          // Identifier of the custom field
            preset:
            {
                "ID": 27    // Identifier of the requisite template
            }
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
        'crm.requisite.preset.field.get',
        [
            'ID' => 1,
            'preset' => ['ID' => 27]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "ID": 1,
        "FIELD_NAME": "RQ_NAME",
        "FIELD_TITLE": "TEST",
        "IN_SHORT_LIST": "N",
        "SORT": 580
    },
    "time": {
        "start": 1716826213.057061,
        "finish": 1716826213.541336,
        "duration": 0.48427510261535645,
        "processing": 0.025674104690551758,
        "date_start": "2024-05-27T18:10:13+02:00",
        "date_finish": "2024-05-27T18:10:13+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../../data-types.md) | An object containing fields that describe the custom field of the requisite template ||
|| **time**
[`time`](../../../../data-types.md) | Information about the execution time of the request ||
|#

### Fields Describing the Custom Field of the Requisite Template

#|
||  **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../../data-types.md) | Identifier of the field. Created automatically and unique within the template 
|| **FIELD_NAME**
[`string`](../../../../data-types.md) | Name of the field 
|| **FIELD_TITLE**
[`string`](../../../../data-types.md) | Alternative name of the field for the requisite.

The alternative name is displayed in various forms for filling out requisites. Depending on the specific form, the alternative name may or may not be used 
|| **SORT**
[`integer`](../../../../data-types.md) | Sorting. Order in the list of template fields 
|| **IN_SHORT_LIST**
[`char`](../../../../data-types.md) | Show in the short list. Deprecated field, currently not used. Retained for backward compatibility. Can take values `Y` or `N` 
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "The PresetField with ID '1' is not found"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|  
|| **Code** | **Description** ||
|| `The PresetField with ID '1' is not found` | The custom field of the template with the specified identifier was not found ||
|| `The Preset with ID '27' is not found` | The template with the specified identifier was not found ||
|| `Access denied` | Insufficient access permissions to retrieve the custom field of the template ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-preset-field-add.md)
- [{#T}](./crm-requisite-preset-field-update.md)
- [{#T}](./crm-requisite-preset-field-available-to-add.md)
- [{#T}](./crm-requisite-preset-field-list.md)
- [{#T}](./crm-requisite-preset-field-delete.md)
- [{#T}](./crm-requisite-preset-field-fields.md)