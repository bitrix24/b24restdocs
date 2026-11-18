# Get Custom Lead Field by ID crm.lead.userfield.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: user with read access to leads

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use the REST 3.0 method [crm.lead.field.custom.get](../../rest-v3/fields/crm-entity-field-custom-get-rest-v3.md).

{% endnote %}

The method `crm.lead.userfield.get` returns a custom lead field by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Positive identifier of the lead custom field.

The identifier can be obtained using the methods [crm.lead.userfield.add](./crm-lead-userfield-add.md) or [crm.lead.userfield.list](./crm-lead-userfield-list.md) ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":399}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.userfield.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":399,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.lead.userfield.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (a representative subset of the user field properties)
    type CrmLeadUserField = {
      ID: string
      ENTITY_ID: string
      FIELD_NAME: string
      USER_TYPE_ID: string
      XML_ID: string | null
      SORT: string
      MULTIPLE: string
      MANDATORY: string
      SHOW_IN_LIST: string
      EDIT_FORM_LABEL?: Record<string, string | null>
      LIST_COLUMN_LABEL?: Record<string, string | null>
    }

    try {
      const response = await $b24.actions.v2.call.make<CrmLeadUserField>({
        method: 'crm.lead.userfield.get',
        params: {
          id: 399,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('User field:', result.FIELD_NAME, result.USER_TYPE_ID)
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
      async function getLeadUserField() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.lead.userfield.get',
            params: {
              id: 399,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('User field:', result.FIELD_NAME, result.USER_TYPE_ID)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getLeadUserField)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.lead.userfield.get(
            bitrix_id=399,
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

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.lead.userfield.get',
                [
                    'id' => 399,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.lead.userfield.get',
        {
            id: 399,
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.lead.userfield.get',
        [
            'id' => 399
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.lead.userfield.get", b24.Params{
    	"id": 399,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.lead.userfield.get: %w", err)
    }

    var item struct {
    	ID         b24.ID `json:"ID"`
    	EntityID   string `json:"ENTITY_ID"`
    	FieldName  string `json:"FIELD_NAME"`
    	UserTypeID string `json:"USER_TYPE_ID"`
    	Sort       string `json:"SORT"`
    	Multiple   string `json:"MULTIPLE"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.EntityID)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "ID": "399",
        "ENTITY_ID": "CRM_LEAD",
        "FIELD_NAME": "UF_CRM_HELLO_WORLD",
        "USER_TYPE_ID": "string",
        "XML_ID": null,
        "SORT": "2000",
        "MULTIPLE": "Y",
        "MANDATORY": "N",
        "SHOW_FILTER": "N",
        "SHOW_IN_LIST": "Y",
        "EDIT_IN_LIST": "N",
        "IS_SEARCHABLE": "N",
        "SETTINGS": {
            "SIZE": 20,
            "ROWS": 10,
            "REGEXP": "",
            "MIN_LENGTH": 0,
            "MAX_LENGTH": 0,
            "DEFAULT_VALUE": "Hello, World! Default value (changed)"
        },
        "EDIT_FORM_LABEL": {
            "en": "Hello, World! Edit (changed)",
            "de": "Hallo, Welt! Bearbeiten (geändert)"
        },
        "LIST_COLUMN_LABEL": {
            "en": "Hello, World! Column (changed)",
            "de": "Hallo, Welt! Spalte (geändert)"
        },
        "LIST_FILTER_LABEL": {
            "en": "Hello, World! Filter (changed)",
            "de": "Hello, World! Filter (changed)"
        },
        "ERROR_MESSAGE": {
            "en": "Hello, World! Error (changed)",
            "de": "Hallo, Welt! Fehler (geändert)"
        },
        "HELP_MESSAGE": {
            "en": "Hello, World! Help (changed)",
            "de": "Hallo, Welt! Hilfe (geändert)"
        }
    },
    "time": {
        "start": 1753790529.430936,
        "finish": 1753790529.487882,
        "duration": 0.05694580078125,
        "processing": 0.0039789676666259766,
        "date_start": "2025-07-29T15:02:09+02:00",
        "date_finish": "2025-07-29T15:02:09+02:00",
        "operating_reset_at": 1753791129,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Custom field description [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

### Result Description {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
```integer | string``` | Field identifier ||
|| **ENTITY_ID**
[`string`](../../../data-types.md) | Field object: `CRM_LEAD` ||
|| **FIELD_NAME**
[`string`](../../../data-types.md) | Full field code with the `UF_CRM_` prefix ||
|| **USER_TYPE_ID**
[`string`](../../../data-types.md) | Field type identifier. The available types are returned by [crm.userfield.types](../../universal/user-defined-fields/crm-userfield-types.md) ||
|| **XML_ID**
```string | null``` | External code ||
|| **SORT**
```integer | string``` | Sort index ||
|| **MULTIPLE**
[`char`](../../../data-types.md) | Multiple values: `Y` — yes, `N` — no ||
|| **MANDATORY**
[`char`](../../../data-types.md) | Required: `Y` — yes, `N` — no ||
|| **SHOW_FILTER**
[`char`](../../../data-types.md) | Filter mode: `N` — do not show, `I` — exact match, `E` — mask, `S` — substring ||
|| **SHOW_IN_LIST**
[`char`](../../../data-types.md) | Show in the list: `Y` — yes, `N` — no ||
|| **EDIT_IN_LIST**
[`char`](../../../data-types.md) | Allow editing in the list: `Y` — yes, `N` — no ||
|| **IS_SEARCHABLE**
[`char`](../../../data-types.md) | Field values are included in search: `Y` — yes, `N` — no ||
|| **SETTINGS**
```object | array | null``` | Settings depend on the field type. If there are no settings, an empty array or `null` may be returned. The list of settings is returned by [crm.userfield.settings.fields](../../universal/user-defined-fields/crm-userfield-settings-fields.md) ||
|| **EDIT_FORM_LABEL**
[`object`](../../../data-types.md) | Edit form labels by language ||
|| **LIST_COLUMN_LABEL**
[`object`](../../../data-types.md) | List column headers by language ||
|| **LIST_FILTER_LABEL**
[`object`](../../../data-types.md) | Filter labels by language ||
|| **ERROR_MESSAGE**
[`object`](../../../data-types.md) | Error messages by language ||
|| **HELP_MESSAGE**
[`object`](../../../data-types.md) | Help messages by language ||
|| **LIST**
[`object[]`](../../../data-types.md) | Only for `enumeration`: list options [(detailed description)](#list) ||
|| **USER_TYPE_OWNER**
```string | null``` | Only for an application field type: identifier of the application that owns the type. If the application is not found, `null` is returned ||
|#

Language labels are objects with [language codes](../../data-types.md#lang-ids) as keys and strings or `null` as values. For example, `{"en": "Label", "de": "Bezeichnung"}`. If labels are not set, the corresponding properties may be missing.

### LIST Item {#list}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
```integer | string``` | Option identifier ||
|| **SORT**
```integer | string``` | Sort index. Options are sorted by `SORT` in ascending order ||
|| **VALUE**
[`string`](../../../data-types.md) | Option text ||
|| **DEF**
[`char`](../../../data-types.md) | Default value: `Y` — yes, `N` — no ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "ID is not defined or invalid."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | `Access denied` | Occurs when:
- the user does not have permission to read leads
- the user attempts to retrieve a custom field not associated with leads ||
|| `400` | Empty value | `ID is not defined or invalid` | The `id` parameter is not provided, or its value after conversion to an integer is less than or equal to zero ||
|| `400` | `ERROR_NOT_FOUND` | `The entity with ID 'id' is not found` | The custom field with the provided `id` was not found ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-lead-userfield-add.md)
- [{#T}](./crm-lead-userfield-update.md)
- [{#T}](./crm-lead-userfield-list.md)
- [{#T}](./crm-lead-userfield-delete.md)
