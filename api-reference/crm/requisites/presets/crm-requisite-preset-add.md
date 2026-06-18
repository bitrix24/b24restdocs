# Create a Template crm.requisite.preset.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method creates a new requisites template.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | A set of fields — an object of the form `{"field": "value"[, ...]}` for adding the template ||
|#

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_TYPE_ID***
[`integer`](../../../data-types.md) | Identifier of the parent object's type. Currently, this is only "Requisite" (identifier `8`).

The identifiers of CRM object types are provided by the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) 
||
|| **COUNTRY_ID***
[`integer`](../../../data-types.md) | Identifier of the country corresponding to the set of fields in the requisites template (for available values, see the method [crm.requisite.preset.countries](./crm-requisite-preset-countries.md)) ||
|| **NAME***
[`string`](../../../data-types.md) | Name of the requisite ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key. Used for exchange operations. Identifier of the external information base object. 

The purpose of the field may change by the final developer. 

Each application ensures the uniqueness of values in this field. It is recommended to use a unique prefix to avoid collisions with other applications. 

Values of the form `#CRM_REQUISITE_PRESET_DEF_...` are reserved in CRM for identifying templates that are used by default. These identifiers should not be used for your purposes, as this may lead to logic violations ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Activity status. Values `Y` or `N` are used. Determines the availability of the template in the selection list when adding requisites ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ENTITY_TYPE_ID":8,"COUNTRY_ID":1,"NAME":"IP","XML_ID":"EXAMPLE_COMPANY__VALUE_1","ACTIVE":"Y","SORT":520}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.preset.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ENTITY_TYPE_ID":8,"COUNTRY_ID":1,"NAME":"IP","XML_ID":"EXAMPLE_COMPANY__VALUE_1","ACTIVE":"Y","SORT":520},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.preset.add
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
        method: 'crm.requisite.preset.add',
        params: {
          fields: {
            ENTITY_TYPE_ID: 8,    // For a requisite preset, always use "Requisite" (ID 8), see crm.enum.ownertype
            COUNTRY_ID: 1,        // Russia
            NAME: 'Sole Proprietor',
            XML_ID: 'EXAMPLE_COMPANY__VALUE_1',    // Unique external identifier
            ACTIVE: 'Y',
            SORT: 520,            // Position in the preset list
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created preset with ID:', result)
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
      async function addRequisitePreset() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.requisite.preset.add',
            params: {
              fields: {
                ENTITY_TYPE_ID: 8,    // For a requisite preset, always use "Requisite" (ID 8), see crm.enum.ownertype
                COUNTRY_ID: 1,        // Russia
                NAME: 'Sole Proprietor',
                XML_ID: 'EXAMPLE_COMPANY__VALUE_1',    // Unique external identifier
                ACTIVE: 'Y',
                SORT: 520,            // Position in the preset list
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
          console.info('Created preset with ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addRequisitePreset)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.requisite.preset.add',
                [
                    'fields' => [
                        'ENTITY_TYPE_ID' => 8,
                        'COUNTRY_ID'     => 1,
                        'NAME'           => 'IP',
                        'XML_ID'         => 'EXAMPLE_COMPANY__VALUE_1',
                        'ACTIVE'         => 'Y',
                        'SORT'           => 520,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Template created with ID ' . $result;
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error creating template: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.requisite.preset.add",
        {
            fields:
            {
                "ENTITY_TYPE_ID": 8,    // For the requisites template, "Requisite" (identifier 8) is always specified, see crm.enum.ownertype
                "COUNTRY_ID": 1,        // USA
                "NAME": "IP",
                "XML_ID": "EXAMPLE_COMPANY__VALUE_1",    // Unique external identifier
                "ACTIVE": "Y",
                "SORT": 520    // Order in the list of templates
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("Template created with ID " + result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.preset.add',
        [
            'fields' =>
            [
                'ENTITY_TYPE_ID' => 8,
                'COUNTRY_ID' => 1,
                'NAME' => 'IP',
                'XML_ID' => 'EXAMPLE_COMPANY__VALUE_1',
                'ACTIVE' => 'Y',
                'SORT' => 520
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
    "result": 347,
    "time": {
        "start": 1716543593.35189,
        "finish": 1716543593.683898,
        "duration": 0.33200788497924805,
        "processing": 0.016175031661987305,
        "date_start": "2024-05-24T11:39:53+02:00",
        "date_finish": "2024-05-24T11:39:53+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Identifier of the created requisites template ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "ENTITY_TYPE_ID is not defined or invalid"
}
```
{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ENTITY_TYPE_ID is not defined or invalid` | The identifier of the parent object's type is not defined or has an invalid value ||
|| `Access denied` | Insufficient access permissions to add the template ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-preset-update.md)
- [{#T}](./crm-requisite-preset-countries.md)
- [{#T}](./crm-requisite-preset-get.md)
- [{#T}](./crm-requisite-preset-list.md)
- [{#T}](./crm-requisite-preset-delete.md)
- [{#T}](./crm-requisite-preset-fields.md)