# Update Custom Field of a Given Requisite Template crm.requisite.preset.field.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

Updates a custom field in the specified template.

## Method Parameters

{% include [Note on parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../../data-types.md) | Identifier of the custom field to be updated. 

Identifiers of custom fields in the requisite template can be obtained using the [crm.requisite.preset.field.list](./crm-requisite-preset-field-list.md) method. ||
|| **preset***
[`object`](../../../../data-types.md) | An object containing the identifier of the template to which the custom field is added (e.g., `{"ID": 27}`).

Template identifiers can be obtained using the method [crm.requisite.preset.list](../crm-requisite-preset-list.md) ||
|| **fields***
[`object`](../../../../data-types.md) | An object with fields and their values that need to be updated. The [crm.requisite.preset.field.fields](./crm-requisite-preset-field-fields.md) method allows you to get the description of the fields that can be modified. 

The API requires a value to be specified in the **FIELD_NAME** field. If it does not need to be changed, the current value can be specified. ||
|#

### Parameter fields

{% include [Note on parameters](../../../../../_includes/required.md) %}

#|
||  **Name**
`type` | **Description** ||
|| **FIELD_NAME***
[`string`](../../../../data-types.md) | Field name. 

The API requires specifying a value in this field. If it does not need to be changed, you can specify the current ||
|| **FIELD_TITLE**
[`string`](../../../../data-types.md) | An alternative name for the field in the requisite.

The alternative name is displayed in various forms for filling out requisites. Depending on the specific form, the alternative name may or may not be used 
||
|| **SORT**
[`integer`](../../../../data-types.md) | Sorting. The order in the list of template fields ||
|| **IN_SHORT_LIST**
[`char`](../../../../data-types.md) | Show in the short list. Deprecated field, currently not used. Retained for backward compatibility. Can take values `Y` or `N` ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":1,"preset":{"ID":27},"fields":{"FIELD_NAME":"RQ_NAME","FIELD_TITLE":"Name","IN_SHORT_LIST":"Y"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.preset.field.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":1,"preset":{"ID":27},"fields":{"FIELD_NAME":"RQ_NAME","FIELD_TITLE":"Name","IN_SHORT_LIST":"Y"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.preset.field.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'crm.requisite.preset.field.update',
        params: {
          ID: 1,
          preset: {
            ID: 27,
          },
          fields: {
            FIELD_NAME: 'RQ_NAME',
            FIELD_TITLE: 'Name',
            IN_SHORT_LIST: 'Y',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Field updated:', result)
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
      async function updatePresetField() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.requisite.preset.field.update',
            params: {
              ID: 1,
              preset: {
                ID: 27,
              },
              fields: {
                FIELD_NAME: 'RQ_NAME',
                FIELD_TITLE: 'Name',
                IN_SHORT_LIST: 'Y',
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
          console.info('Field updated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updatePresetField)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.requisite.preset.field.update',
                [
                    'ID'     => 1,
                    'preset' => [
                        'ID' => 27,
                    ],
                    'fields' => [
                        'FIELD_NAME'    => 'RQ_NAME',
                        'FIELD_TITLE'   => 'Name',
                        'IN_SHORT_LIST' => 'Y',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // The data processing logic you need
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating preset field: ' . $e->getMessage();
    }
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.requisite.preset.field.update(
            bitrix_id=1,
            preset={
                "ID": 27,
            },
            fields={
                "FIELD_NAME": "RQ_NAME",
                "FIELD_TITLE": "Name",
                "IN_SHORT_LIST": "Y",
            },
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.requisite.preset.field.update",
        {
            ID: 1,          // Custom field ID to be changed
            preset:
            {
                "ID": 27    // Requisition template ID
            },
            fields:         // Field values to be changed
            {
                "FIELD_NAME": "RQ_NAME",    // The API requires a value to be specified in this field. If
                                            // no change is needed, leave the previous value.
                "FIELD_TITLE": "Name",
                "IN_SHORT_LIST": "Y",
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.preset.field.update',
        [
            'ID' => 1,
            'preset' => ['ID' => 27],
            'fields' => [
                'FIELD_NAME' => 'RQ_NAME',
                'FIELD_TITLE' => 'Name',
                'IN_SHORT_LIST' => 'Y',
            ]
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
    "result": true,
    "time": {
        "start": 1716898310.398361,
        "finish": 1716898310.936332,
        "duration": 0.537971019744873,
        "processing": 0.09376883506774902,
        "date_start": "2024-05-28T14:11:50+02:00",
        "date_finish": "2024-05-28T14:11:50+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of updating the custom field:
- `true` — updated
- `false` — not updated 
||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "The PresetField with ID '27' is not found"
}
```

{% include notitle [Error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `The PresetField with ID '1' is not found` | The field with the specified identifier was not found. ||
|| `The Preset with ID '27' is not found` | Template with the specified identifier not found ||
|| `ID is not defined or invalid` | The field identifier is not specified or has an invalid value. ||
|| `Access denied` | Insufficient access permissions to delete the field from the requisite template. ||
|#

{% include [System errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-preset-field-add.md)
- [{#T}](./crm-requisite-preset-field-available-to-add.md)
- [{#T}](./crm-requisite-preset-field-get.md)
- [{#T}](./crm-requisite-preset-field-list.md)
- [{#T}](./crm-requisite-preset-field-delete.md)
- [{#T}](./crm-requisite-preset-field-fields.md)