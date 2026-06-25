# Create a New Custom Field For crm.requisite.userfield.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

Creates a new custom field for the company details.

{% note info "Restrictions for the Symbolic Code of the Custom Field" %}

The system limitation for the field name is 20 characters. The custom field name always has the prefix `UF_CRM_`, meaning the actual length of the name is 13 characters.

{% endnote %}

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | A set of fields — an object of the form `{"field": "value"[, ...]}` for adding a custom requisite field ||
|#

### Parameter fields

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_ID***
[`string`](../../../data-types.md) | The identifier of the entity to which the custom field belongs. For requisites, this is always `CRM_REQUISITE` ||
|| **FIELD_NAME***
[`string`](../../../data-types.md) | Symbolic code. For requisites, it always starts with the prefix `UF_CRM_` ||
|| **USER_TYPE_ID***
[`string`](../../../data-types.md) | Data type ([`string`](../../universal/user-defined-fields/crm-userfield-types.md), [`boolean`](../../universal/user-defined-fields/crm-userfield-types.md), [`double`](../../universal/user-defined-fields/crm-userfield-types.md), or [`datetime`](../../universal/user-defined-fields/crm-userfield-types.md)) ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key. Used for exchange operations. Identifier of the object in the external information base. 

The purpose of the field may change by the final developer ||
|| **SORT**
[`int`](../../../data-types.md) | Sorting ||
|| **MULTIPLE**
[`char`](../../../data-types.md) | Multiplicity indicator. Possible values:
- `Y` — yes
- `N` — no

Defaults to `N` 
||
|| **MANDATORY**
[`char`](../../../data-types.md) | Mandatory indicator. Possible values:
- `Y` — yes
- `N` — no

Defaults to `N`
||
|| **SHOW_FILTER**
[`char`](../../../data-types.md) | Show in the list filter. Possible values:
- `N` — do not show
- `I` — exact match
- `E` — mask
- `S` — substring

Defaults to `N` 
||
|| **SHOW_IN_LIST**
[`char`](../../../data-types.md) | Show in the list. Possible values:
- `Y` — yes
- `N` — no

Defaults to `Y` 
||
|| **EDIT_IN_LIST**
[`char`](../../../data-types.md) | Allow user editing. Possible values:
- `Y` — yes
- `N` — no

Defaults to `Y` 
||
|| **IS_SEARCHABLE**
[`char`](../../../data-types.md) | Are field values included in the search. Possible values:
- `Y` — yes
- `N` — no

Defaults to `N` 
||
|| **EDIT_FORM_LABEL**
[`string`](../../../data-types.md) | Label in the edit form ||
|| **LIST_COLUMN_LABEL**
[`string`](../../../data-types.md) | Header in the list ||
|| **LIST_FILTER_LABEL**
[`string`](../../../data-types.md) | Filter label in the list ||
|| **ERROR_MESSAGE**
[`string`](../../../data-types.md) | Error message ||
|| **HELP_MESSAGE**
[`string`](../../../data-types.md) | Help ||
|| **LIST**
[`uf_enum_element`](../../../data-types.md) | List elements. For detailed information, see the section [{#T}](../../universal/user-defined-fields/crm-userfield-enumeration-fields.md) ||
|| **SETTINGS**
[`object`](../../../data-types.md) | Additional settings (dependent on type). For detailed information, see the section [{#T}](../../universal/user-defined-fields/crm-userfield-settings-fields.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"USER_TYPE_ID":"string","ENTITY_ID":"CRM_REQUISITE","SORT":100,"MULTIPLE":"N","MANDATORY":"N","SHOW_FILTER":"E","SHOW_IN_LIST":"Y","EDIT_FORM_LABEL":"PP - Line","LIST_COLUMN_LABEL":"PP - Line","LIST_FILTER_LABEL":"PP - Line","FIELD_NAME":"NEWTECH_v1_STRING"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.userfield.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"USER_TYPE_ID":"string","ENTITY_ID":"CRM_REQUISITE","SORT":100,"MULTIPLE":"N","MANDATORY":"N","SHOW_FILTER":"E","SHOW_IN_LIST":"Y","EDIT_FORM_LABEL":"PP - Line","LIST_COLUMN_LABEL":"PP - Line","LIST_FILTER_LABEL":"PP - Line","FIELD_NAME":"NEWTECH_v1_STRING"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.userfield.add
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
        method: 'crm.requisite.userfield.add',
        params: {
          fields: {
            USER_TYPE_ID: 'string',
            ENTITY_ID: 'CRM_REQUISITE',
            SORT: 100,
            MULTIPLE: 'N',
            MANDATORY: 'N',
            SHOW_FILTER: 'E',
            SHOW_IN_LIST: 'Y',
            EDIT_FORM_LABEL: 'PP - String',
            LIST_COLUMN_LABEL: 'PP - String',
            LIST_FILTER_LABEL: 'PP - String',
            FIELD_NAME: 'NEWTECH_v1_STRING',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created user field ID:', result)
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
      async function addRequisiteUserField() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.requisite.userfield.add',
            params: {
              fields: {
                USER_TYPE_ID: 'string',
                ENTITY_ID: 'CRM_REQUISITE',
                SORT: 100,
                MULTIPLE: 'N',
                MANDATORY: 'N',
                SHOW_FILTER: 'E',
                SHOW_IN_LIST: 'Y',
                EDIT_FORM_LABEL: 'PP - String',
                LIST_COLUMN_LABEL: 'PP - String',
                LIST_FILTER_LABEL: 'PP - String',
                FIELD_NAME: 'NEWTECH_v1_STRING',
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
          console.info('Created user field ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addRequisiteUserField)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.requisite.userfield.add',
                [
                    'fields' => [
                        'USER_TYPE_ID'      => 'string',
                        'ENTITY_ID'         => 'CRM_REQUISITE',
                        'SORT'              => 100,
                        'MULTIPLE'          => 'N',
                        'MANDATORY'         => 'N',
                        'SHOW_FILTER'       => 'E',
                        'SHOW_IN_LIST'      => 'Y',
                        'EDIT_FORM_LABEL'   => 'PP - Line',
                        'LIST_COLUMN_LABEL' => 'PP - Line',
                        'LIST_FILTER_LABEL' => 'PP - Line',
                        'FIELD_NAME'        => 'NEWTECH_v1_STRING',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding user field: ' . $e->getMessage();
    }
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.requisite.userfield.add(
            fields={
                "USER_TYPE_ID": "string",
                "ENTITY_ID": "CRM_REQUISITE",
                "SORT": 100,
                "MULTIPLE": "N",
                "MANDATORY": "N",
                "SHOW_FILTER": "E",
                "SHOW_IN_LIST": "Y",
                "EDIT_FORM_LABEL": "PP - Line",
                "LIST_COLUMN_LABEL": "PP - Line",
                "LIST_FILTER_LABEL": "PP - Line",
                "FIELD_NAME": "NEWTECH_v1_STRING",
            },
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.requisite.userfield.add",
        {
            fields:
            {
              "USER_TYPE_ID": "string",
              "ENTITY_ID": "CRM_REQUISITE",
              "SORT": 100,
              "MULTIPLE": "N",
              "MANDATORY": "N",
              "SHOW_FILTER": "E",
              "SHOW_IN_LIST": "Y",
              "EDIT_FORM_LABEL": "PP - Line",
              "LIST_COLUMN_LABEL": "PP - Line",
              "LIST_FILTER_LABEL": "PP - Line",
              "FIELD_NAME": "NEWTECH_v1_STRING"
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
        'crm.requisite.userfield.add',
        [
            'fields' => [
                'USER_TYPE_ID' => 'string',
                'ENTITY_ID' => 'CRM_REQUISITE',
                'SORT' => 100,
                'MULTIPLE' => 'N',
                'MANDATORY' => 'N',
                'SHOW_FILTER' => 'E',
                'SHOW_IN_LIST' => 'Y',
                'EDIT_FORM_LABEL' => 'PP - Line',
                'LIST_COLUMN_LABEL' => 'PP - Line',
                'LIST_FILTER_LABEL' => 'PP - Line',
                'FIELD_NAME' => 'NEWTECH_v1_STRING'
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
    "result": 235,
    "time": {
        "start": 1717681176.885836,
        "finish": 1717681177.353738,
        "duration": 0.46790218353271484,
        "processing": 0.084564208984375,
        "date_start": "2024-06-06T15:39:36+02:00",
        "date_finish": "2024-06-06T15:39:37+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Identifier of the created custom field ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "ERROR_CORE",
    "error_description": "The field UF_CRM_NEWTECH_V1_STRING for the CRM_REQUISITE object already exists."
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|
|| **Code** | **Error text** | **Description** ||
|| `ERROR_CORE` | `The field UF_CRM_NEWTECH_V1_STRING for the CRM_REQUISITE object already exists` | Attempt to recreate a custom field with the same symbolic code ||
|| Empty string | `The 'USER_TYPE_ID' field is not found` | Data type for the custom field is not specified ||
|| Empty string | `The 'FIELD_NAME' field is not found` | Symbolic code for the custom field is not specified ||
|| Empty string | `Access denied` | Insufficient access permissions to add a custom field ||
|| `ERROR_CORE` | `Fail to create new user field` | Failed to create a custom field ||
|| `ERROR_CORE` | `Fail to save enumeration field values` | Failed to save values for the custom list-type field (e.g., when there is a duplication of the external key of one of the values) ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-userfield-update.md)
- [{#T}](./crm-requisite-userfield-get.md)
- [{#T}](./crm-requisite-userfield-list.md)
- [{#T}](./crm-requisite-userfield-delete.md)