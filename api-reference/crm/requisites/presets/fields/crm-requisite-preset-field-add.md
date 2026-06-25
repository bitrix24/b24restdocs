# Add a Custom Field to the CRM Requisite Template crm.requisite.preset.field.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

Adds a custom field to a company details template. Use the [crm.requisite.preset.field.availabletoadd](./crm-requisite-preset-field-available-to-add.md) method to retrieve the fields available for addition to the template.

Before adding a user-defined field `UF_...` to the template, you must create it using the [crm.requisite.userfield.add](../../user-fields/crm-requisite-userfield-add.md) method or ensure that it already exists.

## Method Parameters

{% include [Note on parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **preset***
[`object`](../../../../data-types.md) | An object containing the identifier of the template to which the custom field is added (e.g., `{"ID": 27}`).

Template identifiers can be obtained using the method [crm.requisite.preset.list](../crm-requisite-preset-list.md) ||
|| **fields***
[`object`](../../../../data-types.md) | A set of fields — an object of the form `{"field": "value"[, ...]}` for adding the custom field to the template ||
|#

### Parameter fields

{% include [Note on parameters](../../../../../_includes/required.md) %}

#|
||  **Name**
`type` | **Description** ||
|| **FIELD_NAME***
[`string`](../../../../data-types.md) | The name of the field. ||
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
    -d '{"preset":{"ID":27},"fields":{"FIELD_NAME":"RQ_NAME","FIELD_TITLE":"TEST","IN_SHORT_LIST":"N","SORT":580}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.preset.field.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"preset":{"ID":27},"fields":{"FIELD_NAME":"RQ_NAME","FIELD_TITLE":"TEST","IN_SHORT_LIST":"N","SORT":580},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.preset.field.add
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
        method: 'crm.requisite.preset.field.add',
        params: {
          preset: {
            ID: 27, // Template ID
          },
          fields: {
            FIELD_NAME: 'RQ_NAME',
            FIELD_TITLE: 'TEST',
            IN_SHORT_LIST: 'N',
            SORT: 580,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Added field ID:', result)
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
      async function addPresetField() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.requisite.preset.field.add',
            params: {
              preset: {
                ID: 27, // Template ID
              },
              fields: {
                FIELD_NAME: 'RQ_NAME',
                FIELD_TITLE: 'TEST',
                IN_SHORT_LIST: 'N',
                SORT: 580,
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
          console.info('Added field ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addPresetField)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.requisite.preset.field.add',
                [
                    'preset' => [
                        'ID' => 27, // Template ID
                    ],
                    'fields' => [ // Object with custom field description
                        'FIELD_NAME'    => 'RQ_NAME',
                        'FIELD_TITLE'   => 'TEST',
                        'IN_SHORT_LIST' => 'N',
                        'SORT'          => 580,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Custom field with ID has been added to the template ' . $result;
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding custom field: ' . $e->getMessage();
    }
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.requisite.preset.field.add(
            preset={
                "ID": 27,
            },
            fields={
                "FIELD_NAME": "RQ_NAME",
                "FIELD_TITLE": "TEST",
                "IN_SHORT_LIST": "N",
                "SORT": 580,
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
        "crm.requisite.preset.field.add",
        {
            preset:
            {
                "ID": 27    // Template ID
            },
            fields:        // Object with custom field description
            {
                "FIELD_NAME": "RQ_NAME",
                "FIELD_TITLE": "TEST",
                "IN_SHORT_LIST": "N",
                "SORT": 580
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("Custom field with ID has been added to the template " + result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.preset.field.add',
        [
            'preset' => ['ID' => 27],
            'fields' => [
                'FIELD_NAME' => 'RQ_NAME',
                'FIELD_TITLE' => 'TEST',
                'IN_SHORT_LIST' => 'N',
                'SORT' => 580
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
    "result": 3,
    "time": {
        "start": 1716811635.108203,
        "finish": 1716811635.503093,
        "duration": 0.39489006996154785,
        "processing": 0.043524980545043945,
        "date_start": "2024-05-27T14:07:15+02:00",
        "date_finish": "2024-05-27T14:07:15+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../../data-types.md) | Identifier of the added field ||
|| **time**
[`time`](../../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "The field 'RQ_NAME' can not be added."
}
```

{% include notitle [Error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `The field 'RQ_NAME' can not be added` | The field cannot be added. The field may already exist in the template or it is not available for the country to which the template belongs ||
|| `The Preset with ID '27' is not found` | Template with the specified identifier not found ||
|| `Access denied` | Insufficient access permissions to add the custom field to the requisite template ||
|#

{% include [System errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-preset-field-update.md)
- [{#T}](./crm-requisite-preset-field-available-to-add.md)
- [{#T}](./crm-requisite-preset-field-get.md)
- [{#T}](./crm-requisite-preset-field-list.md)
- [{#T}](./crm-requisite-preset-field-delete.md)
- [{#T}](./crm-requisite-preset-field-fields.md)