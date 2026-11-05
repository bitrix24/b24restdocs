# Get Custom Field of Estimates by ID crm.quote.userfield.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: user with read access to estimates

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use the REST 3.0 method [crm.quote.field.custom.get](../../rest-v3/fields/crm-entity-field-custom-get-rest-v3.md).

{% endnote %}

The method `crm.quote.userfield.get` returns a custom estimate field description by its identifier: name, type, settings, and labels in all languages.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the custom estimate field.

The identifier can be obtained using the methods [crm.quote.userfield.add](./crm-quote-user-field-add.md) or [crm.quote.userfield.list](./crm-quote-user-field-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":399}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.userfield.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":399,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.quote.userfield.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type QuoteUserFieldGetResult = {
      ID: string
      ENTITY_ID: string
      FIELD_NAME: string
      USER_TYPE_ID: string
      XML_ID: string | null
      SORT: string
      MULTIPLE: string
      MANDATORY: string
      SHOW_FILTER: string
      SHOW_IN_LIST: string
      EDIT_IN_LIST: string
      IS_SEARCHABLE: string
      SETTINGS: {
        SIZE: number
        ROWS: number
        REGEXP: string
        MIN_LENGTH: number
        MAX_LENGTH: number
        DEFAULT_VALUE: string
      }
      EDIT_FORM_LABEL: Record<string, string>
      LIST_COLUMN_LABEL: Record<string, string>
      LIST_FILTER_LABEL: Record<string, string>
      ERROR_MESSAGE: Record<string, string>
      HELP_MESSAGE: Record<string, string>
    }

    try {
      const response = await $b24.actions.v2.call.make<QuoteUserFieldGetResult>({
        method: 'crm.quote.userfield.get',
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
        console.info(result.ID, result.FIELD_NAME, result.USER_TYPE_ID)
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
      async function getQuoteUserField() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.quote.userfield.get',
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
          console.info(result.ID, result.FIELD_NAME, result.USER_TYPE_ID)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getQuoteUserField)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.quote.userfield.get(bitrix_id=399).response
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

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.quote.userfield.get',
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
        'crm.quote.userfield.get',
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
        'crm.quote.userfield.get',
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
    res, err := client.Core().Call(ctx, "crm.quote.userfield.get", b24.Params{
    	"id": 399,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.quote.userfield.get: %w", err)
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

HTTP status: **200**

```json
{
    "result": {
        "ID": "399",
        "ENTITY_ID": "CRM_QUOTE",
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
            "DEFAULT_VALUE": "Hello, world! Default value (changed)"
        },
        "EDIT_FORM_LABEL": {
            "ar": "",
            "br": "",
            "de": "Hallo, Welt! Bearbeiten (geändert)",
            "en": "Hello, World! Edit (changed)",
            "fr": "",
            "hi": "",
            "id": "",
            "in": "",
            "it": "",
            "ja": "",
            "kz": "",
            "la": "",
            "ms": "",
            "pl": "",
            "sc": "",
            "tc": "",
            "th": "",
            "tr": "",
            "ua": "",
            "vn": ""
        },
        "LIST_COLUMN_LABEL": {
            "ar": "",
            "br": "",
            "de": "Hallo, Welt! Spalte (geändert)",
            "en": "Hello, World! Column (changed)",
            "fr": "",
            "hi": "",
            "id": "",
            "in": "",
            "it": "",
            "ja": "",
            "kz": "",
            "la": "",
            "ms": "",
            "pl": "",
            "sc": "",
            "tc": "",
            "th": "",
            "tr": "",
            "ua": "",
            "vn": ""
        },
        "LIST_FILTER_LABEL": {
            "ar": "Hello, world! Filter (changed)",
            "br": "Hello, world! Filter (changed)",
            "de": "Hello, world! Filter (changed)",
            "en": "Hello, world! Filter (changed)",
            "fr": "Hello, world! Filter (changed)",
            "hi": "Hello, world! Filter (changed)",
            "id": "Hello, world! Filter (changed)",
            "in": "Hello, world! Filter (changed)",
            "it": "Hello, world! Filter (changed)",
            "ja": "Hello, world! Filter (changed)",
            "kz": "Hello, world! Filter (changed)",
            "la": "Hello, world! Filter (changed)",
            "ms": "Hello, world! Filter (changed)",
            "pl": "Hello, world! Filter (changed)",
            "sc": "Hello, world! Filter (changed)",
            "tc": "Hello, world! Filter (changed)",
            "th": "Hello, world! Filter (changed)",
            "tr": "Hello, world! Filter (changed)",
            "ua": "Hello, world! Filter (changed)",
            "vn": "Hello, world! Filter (changed)"
        },
        "ERROR_MESSAGE": {
            "ar": "",
            "br": "",
            "de": "Hallo, Welt! Fehler (geändert)",
            "en": "Hello, World! Error (changed)",
            "fr": "",
            "hi": "",
            "id": "",
            "in": "",
            "it": "",
            "ja": "",
            "kz": "",
            "la": "",
            "ms": "",
            "pl": "",
            "sc": "",
            "tc": "",
            "th": "",
            "tr": "",
            "ua": "",
            "vn": ""
        },
        "HELP_MESSAGE": {
            "ar": "",
            "br": "",
            "de": "Hallo, Welt! Hilfe (geändert)",
            "en": "Hello, World! Help (changed)",
            "fr": "",
            "hi": "",
            "id": "",
            "in": "",
            "it": "",
            "ja": "",
            "kz": "",
            "la": "",
            "ms": "",
            "pl": "",
            "sc": "",
            "tc": "",
            "th": "",
            "tr": "",
            "ua": "",
            "vn": ""
        }
    },
    "time": {
        "start": 1753790529.430936,
        "finish": 1753790529.487882,
        "duration": 0.05694580078125,
        "processing": 0.0039789676666259766,
        "date_start": "2025-07-29T15:02:09+03:00",
        "date_finish": "2025-07-29T15:02:09+03:00",
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

#### result Object {#result}

Numeric values of `ID` and `SORT` are returned as strings.

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../../data-types.md) | Custom field identifier ||
|| **ENTITY_ID**
[`string`](../../../data-types.md) | The object the field belongs to. For estimates, always `CRM_QUOTE` ||
|| **FIELD_NAME**
[`string`](../../../data-types.md) | Full field name with the `UF_CRM_` prefix. The universal methods [crm.item.update](../../universal/crm-item-update.md) and [crm.item.get](../../universal/crm-item-get.md) with `entityTypeId = 7` write and read the field value. In these methods, the field name is written in camelCase: `UF_CRM_MANAGER_NOTE` becomes `ufCrmManagerNote`. The methods [crm.quote.update](../crm-quote-update.md) and [crm.quote.get](../crm-quote-get.md) work with the full name, but their development has been halted. The method `crm.quote.userfield.get` does not return field values in estimates ||
|| **USER_TYPE_ID**
[`string`](../../../data-types.md) | Field type. For the list of values, see the `USER_TYPE_ID` parameter of the method [crm.quote.userfield.add](./crm-quote-user-field-add.md#parameter-fields) ||
|| **USER_TYPE_OWNER**
[`string`](../../../data-types.md) | Identifier of the application that registered the [custom field type](../../universal/user-defined-fields/userfield-type.md). Returned only for fields of a custom type ||
|| **XML_ID**
[`string`](../../../data-types.md) | External code. If not set, `null` ||
|| **SORT**
[`string`](../../../data-types.md) | Sort index ||
|| **MULTIPLE**
[`char`](../../../data-types.md) | Whether the field is multiple: `Y` or `N` ||
|| **MANDATORY**
[`char`](../../../data-types.md) | Whether the field is required: `Y` or `N` ||
|| **SHOW_FILTER**
[`char`](../../../data-types.md) | Field mode in the list filter:
- `N` — the field is not in the filter
- `I` — exact match search
- `E` — mask search
- `S` — substring search ||
|| **SHOW_IN_LIST**
[`char`](../../../data-types.md) | Whether to show the field in the list: `Y` or `N` ||
|| **EDIT_IN_LIST**
[`char`](../../../data-types.md) | Whether the user can edit the field: `Y` or `N` ||
|| **IS_SEARCHABLE**
[`char`](../../../data-types.md) | Whether the field values are included in search: `Y` or `N` ||
|| **SETTINGS**
[`object`](../../../data-types.md) | Field type settings. Besides the settings accepted by [crm.quote.userfield.add](./crm-quote-user-field-add.md#settings), it may contain service keys, for example, `SIZE`, `MIN_LENGTH`, and `MAX_LENGTH` for the `string` type. The method [crm.userfield.settings.fields](../../universal/user-defined-fields/crm-userfield-settings-fields.md) returns the list of type settings ||
|| **EDIT_FORM_LABEL**
[`lang_map`](../../data-types.md#lang-ids) | Label in the edit form in each language ||
|| **LIST_COLUMN_LABEL**
[`lang_map`](../../data-types.md#lang-ids) | Column header in the list in each language ||
|| **LIST_FILTER_LABEL**
[`lang_map`](../../data-types.md#lang-ids) | Filter label in each language ||
|| **ERROR_MESSAGE**
[`lang_map`](../../data-types.md#lang-ids) | Error message in each language ||
|| **HELP_MESSAGE**
[`lang_map`](../../data-types.md#lang-ids) | Help text in each language ||
|| **LIST**
[`object[]`](../../../data-types.md) | List items. Returned only for `enumeration` fields, sorted by `SORT`. Each item contains:
- `ID` — item identifier; pass it as the field value when writing to an estimate
- `SORT` — sort index
- `VALUE` — item value
- `DEF` — whether the item is the default: `Y` or `N`

The method does not return the item's external code `XML_ID`; the method [crm.quote.userfield.list](./crm-quote-user-field-list.md) returns it ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "ID is not defined or invalid."
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | `ID is not defined or invalid.` | `id` is not provided or is less than one ||
|| `400` | Empty value | `Access denied.` | Occurs when:
- the user does not have read access to estimates
- the field with the provided `id` belongs to another object, for example, leads ||
|| `400` | `ERROR_NOT_FOUND` | `The entity with ID 'id' is not found.` | The custom field with the provided `id` does not exist ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-quote-user-field-add.md)
- [{#T}](./crm-quote-user-field-update.md)
- [{#T}](./crm-quote-user-field-list.md)
- [{#T}](./crm-quote-user-field-delete.md)
- [{#T}](../../universal/crm-item-get.md)
- [{#T}](./index.md)
