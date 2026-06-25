# Update Custom Field of CRM Requisite

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

Updates an existing custom field of the company details.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the user field. Can be obtained using the method [crm.requisite.userfield.list](./crm-requisite-userfield-list.md) ||
|| **fields***
[`object`](../../../data-types.md) | Set of fields — an object of the form `{"field": "value"[, ...]}`, the values of which need to be changed ||
|#

### Parameter fields

#|
|| **Name**
`type` | **Description** ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key. Used for exchange operations. Identifier of the object in the external information base. 

The purpose of the field may change by the final developer ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting ||
|| **MULTIPLE**
[`char`](../../../data-types.md) | Indicator of multiplicity. Possible values:
- `Y` — yes
- `N` — no 
||
|| **MANDATORY**
[`char`](../../../data-types.md) | Indicator of mandatory status. Possible values:
- `Y` — yes
- `N` — no 
||
|| **SHOW_FILTER**
[`char`](../../../data-types.md) | Show in the list filter. Possible values:
- `N` — do not show
- `I` — exact match
- `E` — mask
- `S` — substring 
||
|| **SHOW_IN_LIST**
[`char`](../../../data-types.md) | Whether to show in the list. Possible values:
- `Y` — yes
- `N` — no 
||
|| **EDIT_IN_LIST**
[`char`](../../../data-types.md) | Allow user editing? Possible values:
- `Y` — yes
- `N` — no 
||
|| **IS_SEARCHABLE**
[`char`](../../../data-types.md) | Whether the field values participate in search. Possible values:
- `Y` — yes
- `N` — no 
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
    -d '{"id":235,"fields":{"EDIT_FORM_LABEL":"Category","LIST_COLUMN_LABEL":"Category","LIST_FILTER_LABEL":"Category"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.userfield.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":235,"fields":{"EDIT_FORM_LABEL":"Category","LIST_COLUMN_LABEL":"Category","LIST_FILTER_LABEL":"Category"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.userfield.update
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
        method: 'crm.requisite.userfield.update',
        params: {
          id: 235,
          fields: {
            EDIT_FORM_LABEL: 'Category',
            LIST_COLUMN_LABEL: 'Category',
            LIST_FILTER_LABEL: 'Category',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('User field updated:', result)
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
      async function updateRequisiteUserField() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.requisite.userfield.update',
            params: {
              id: 235,
              fields: {
                EDIT_FORM_LABEL: 'Category',
                LIST_COLUMN_LABEL: 'Category',
                LIST_FILTER_LABEL: 'Category',
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
          console.info('User field updated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateRequisiteUserField)
    </script>
    ```

- PHP

    ```php
    $title = "Category";
    
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.requisite.userfield.update',
                [
                    'id' => 235,
                    'fields' => [
                        'EDIT_FORM_LABEL'   => $title,
                        'LIST_COLUMN_LABEL' => $title,
                        'LIST_FILTER_LABEL' => $title
                    ]
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
        echo 'Error updating user field: ' . $e->getMessage();
    }
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.requisite.userfield.update(
            bitrix_id=235,
            fields={
                "EDIT_FORM_LABEL": "Category",
                "LIST_COLUMN_LABEL": "Category",
                "LIST_FILTER_LABEL": "Category",
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
    const title = "Category";
    BX24.callMethod(
        "crm.requisite.userfield.update",
        {
            id: 235,
            fields:
            {
                "EDIT_FORM_LABEL": title,
                "LIST_COLUMN_LABEL": title,
                "LIST_FILTER_LABEL": title
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

    $title = "Category";

    $result = CRest::call(
        'crm.requisite.userfield.update',
        [
            'id' => 235,
            'fields' =>
            [
                'EDIT_FORM_LABEL' => $title,
                'LIST_COLUMN_LABEL' => $title,
                'LIST_FILTER_LABEL' => $title
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
        "start": 1717769551.504986,
        "finish": 1717769551.817433,
        "duration": 0.31244707107543945,
        "processing": 0.04784202575683594,
        "date_start": "2024-06-07T16:12:31+02:00",
        "date_finish": "2024-06-07T16:12:31+02:00",
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
- true — updated
- false — not updated 
||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "The entity with ID '235' is not found."
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|
|| **Code** | **Error text** | **Description** ||
|| Empty string | `Operation is not allowed. Entity ID is not defined` | The user field with the specified identifier was not found ||
|| Empty string | `The entity with ID '235' is not found` | The user field with the specified identifier was not found ||
|| Empty string | `ID is not defined or invalid` | The identifier of the user field is not specified or has an invalid value ||
|| Empty string | `Access denied` | Insufficient access rights to modify the custom field ||
|| `ERROR_CORE` | `Fail to update user field` |  Failed to update the custom field ||
|| `ERROR_CORE` | `Fail to save enumeration field values` | Failed to save values for the custom list-type field (e.g., when there is a duplication of the external key of one of the values) ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-userfield-add.md)
- [{#T}](./crm-requisite-userfield-get.md)
- [{#T}](./crm-requisite-userfield-list.md)
- [{#T}](./crm-requisite-userfield-delete.md)