# Retrieve a Contact Custom Field by ID crm.contact.userfield.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with "read" access permission for contacts

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use the REST 3.0 method [crm.contact.field.custom.get](../../rest-v3/fields/crm-entity-field-custom-get-rest-v3.md).

{% endnote %}

The method `crm.contact.userfield.get` returns a custom contact field by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the custom field associated with the contact.

The identifier can be obtained using the methods [`crm.contact.userfield.add`](./crm-contact-userfield-add.md) or [`crm.contact.userfield.list`](./crm-contact-userfield-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Get the custom field with `id = 399`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":399}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.userfield.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":399,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.userfield.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CrmContactUserfield = {
      ID: string
      ENTITY_ID: string
      FIELD_NAME: string
      USER_TYPE_ID: string
      XML_ID: string | null
      SORT: string
      MULTIPLE: string
      MANDATORY: string
    }

    try {
      const response = await $b24.actions.v2.call.make<CrmContactUserfield>({
        method: 'crm.contact.userfield.get',
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
        console.info('Userfield:', result.ID, result.FIELD_NAME)
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
      async function getContactUserfield() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.contact.userfield.get',
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
          console.info('Userfield:', result.ID, result.FIELD_NAME)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getContactUserfield)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.contact.userfield.get(bitrix_id=399).response
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
                'crm.contact.userfield.get',
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
        'crm.contact.userfield.get',
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
        'crm.contact.userfield.get',
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
    res, err := client.Core().Call(ctx, "crm.contact.userfield.get", b24.Params{
    	"id": 399,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.contact.userfield.get: %w", err)
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
        "ENTITY_ID": "CRM_CONTACT",
        "FIELD_NAME": "UF_CRM_HELLO_WORLD",
        "USER_TYPE_ID": "string",
        "XML_ID": null,
        "SORT": "1000",
        "MULTIPLE": "Y",
        "MANDATORY": "Y",
        "SHOW_FILTER": "E",
        "SHOW_IN_LIST": "Y",
        "EDIT_IN_LIST": "Y",
        "IS_SEARCHABLE": "Y",
        "SETTINGS": {
            "SIZE": 20,
            "ROWS": 3,
            "REGEXP": "",
            "MIN_LENGTH": 0,
            "MAX_LENGTH": 0,
            "DEFAULT_VALUE": "Hello, world! Default value"
        },
        "EDIT_FORM_LABEL": {
            "ar": "Field 'Hello, world!'",
            "br": "Field 'Hello, world!'",
            "en": "Hello, World! Edit",
            "fr": "Field 'Hello, world!'",
            "id": "Field 'Hello, world!'",
            "it": "Field 'Hello, world!'",
            "ja": "Field 'Hello, world!'",
            "la": "Field 'Hello, world!'",
            "ms": "Field 'Hello, world!'",
            "pl": "Field 'Hello, world!'",
            "de": "Hello, world! Edit",
            "sc": "Field 'Hello, world!'",
            "tc": "Field 'Hello, world!'",
            "th": "Field 'Hello, world!'",
            "tr": "Field 'Hello, world!'",
            "vn": "Field 'Hello, world!'"
        },
        "LIST_COLUMN_LABEL": {
            "ar": "Field 'Hello, world!'",
            "br": "Field 'Hello, world!'",
            "en": "Hello, World! Column",
            "fr": "Field 'Hello, world!'",
            "id": "Field 'Hello, world!'",
            "it": "Field 'Hello, world!'",
            "ja": "Field 'Hello, world!'",
            "la": "Field 'Hello, world!'",
            "ms": "Field 'Hello, world!'",
            "pl": "Field 'Hello, world!'",
            "de": "Hello, world! Column",
            "sc": "Field 'Hello, world!'",
            "tc": "Field 'Hello, world!'",
            "th": "Field 'Hello, world!'",
            "tr": "Field 'Hello, world!'",
            "vn": "Field 'Hello, world!'"
        },
        "LIST_FILTER_LABEL": {
            "ar": "Hello, world! Filter",
            "br": "Hello, world! Filter",
            "en": "Hello, world! Filter",
            "fr": "Hello, world! Filter",
            "id": "Hello, world! Filter",
            "it": "Hello, world! Filter",
            "ja": "Hello, world! Filter",
            "la": "Hello, world! Filter",
            "ms": "Hello, world! Filter",
            "pl": "Hello, world! Filter",
            "de": "Hello, world! Filter",
            "sc": "Hello, world! Filter",
            "tc": "Hello, world! Filter",
            "th": "Hello, world! Filter",
            "tr": "Hello, world! Filter",
            "vn": "Hello, world! Filter"
        },
        "ERROR_MESSAGE": {
            "ar": "Field 'Hello, world!'",
            "br": "Field 'Hello, world!'",
            "en": "Hello, World! Error",
            "fr": "Field 'Hello, world!'",
            "id": "Field 'Hello, world!'",
            "it": "Field 'Hello, world!'",
            "ja": "Field 'Hello, world!'",
            "la": "Field 'Hello, world!'",
            "ms": "Field 'Hello, world!'",
            "pl": "Field 'Hello, world!'",
            "de": "Hello, world! Error",
            "sc": "Field 'Hello, world!'",
            "tc": "Field 'Hello, world!'",
            "th": "Field 'Hello, world!'",
            "tr": "Field 'Hello, world!'",
            "vn": "Field 'Hello, world!'"
        },
        "HELP_MESSAGE": {
            "ar": "Field 'Hello, world!'",
            "br": "Field 'Hello, world!'",
            "en": "Hello, World! Help",
            "fr": "Field 'Hello, world!'",
            "id": "Field 'Hello, world!'",
            "it": "Field 'Hello, world!'",
            "ja": "Field 'Hello, world!'",
            "la": "Field 'Hello, world!'",
            "ms": "Field 'Hello, world!'",
            "pl": "Field 'Hello, world!'",
            "de": "Hello, world! Help",
            "sc": "Field 'Hello, world!'",
            "tc": "Field 'Hello, world!'",
            "th": "Field 'Hello, world!'",
            "tr": "Field 'Hello, world!'",
            "vn": "Field 'Hello, world!'"
        }
    },
    "time": {
        "start": 1724318753.341079,
        "finish": 1724318753.621247,
        "duration": 0.2801680564880371,
        "processing": 0.023567914962768555,
        "date_start": "2024-08-22T11:25:53+02:00",
        "date_finish": "2024-08-22T11:25:53+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`userfield`](#userfield) | Root element of the response, contains information about the custom field ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### userfield Object {#userfield}

Numeric `ID` and `SORT` values are returned as strings, including in `LIST` items.

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../../data-types.md) | Identifier of the custom field ||
|| **ENTITY_ID**
[`string`](../../../data-types.md) | String identifier binding the custom field to the entity.

Always `CRM_CONTACT` for contact fields ||
|| **FIELD_NAME**
[`string`](../../../data-types.md) | Field name with the `UF_CRM_` prefix, unique within contacts. Use this name to write and read the field value in contact methods, such as [crm.contact.update](../crm-contact-update.md) and [crm.contact.get](../crm-contact-get.md). This method does not return field values from contacts ||
|| **USER_TYPE_ID**
[`string`](../../../data-types.md) | Data type of the custom field. Possible values:
- `string` — string
- `integer` — integer
- `double` — number
- `boolean` — yes/no
- `datetime` — date/time
- `date` — date
- `money` — money
- `url` — link
- `address` — address
- `enumeration` — list
- `file` — file
- `employee` — employee binding
- `crm_status` — binding to CRM directory
- `iblock_section` — binding to information block sections
- `iblock_element` — binding to information block elements
- `crm` — binding to CRM elements
- [custom field types](../../universal/user-defined-fields/userfield-type.md)
||
|| **XML_ID**
[`string`](../../../data-types.md) | External code. If not specified, `null` ||
|| **SORT**
[`string`](../../../data-types.md) | Sorting index ||
|| **MULTIPLE**
[`char`](../../../data-types.md) | Indicates whether the field is multiple. Possible values:
- `Y` — yes
- `N` — no
||
|| **MANDATORY**
[`char`](../../../data-types.md) | Is the field mandatory? Possible values:
- `Y` — yes
- `N` — no
||
|| **SHOW_FILTER**
[`char`](../../../data-types.md) | Field display mode in the filter. Possible values:
- `N` — do not show
- `I` — exact match
- `E` — mask
- `S` — substring
||
|| **SHOW_IN_LIST**
[`char`](../../../data-types.md) | Should the user field be shown in the list?

Affects the default set of contact list columns. The user can customize the columns.

Possible values:
- `Y` — yes
- `N` — no
||
|| **EDIT_IN_LIST**
[`char`](../../../data-types.md) | Allows user editing. Possible values:
- `Y` — yes
- `N` — no
||
|| **IS_SEARCHABLE**
[`char`](../../../data-types.md) | Are the field values searchable?

This parameter does not affect anything within `crm`.

Possible values:
- `Y` — yes
- `N` — no
||
|| **SETTINGS**
[`object`](../../../data-types.md) | Additional field parameters depend on `USER_TYPE_ID`. Settings available when creating a field are described in [Parameter SETTINGS](./crm-contact-userfield-add.md#settings). The response may contain additional field type settings ||
|| **LIST**
[`uf_enum_element[]`](#uf_enum_element) | List of possible values. Returned for the `enumeration` field type ||
|| **EDIT_FORM_LABEL**
[`lang_map`](../../data-types.md#lang-ids) | Label in the edit form for all Bitrix24 languages ||
|| **LIST_COLUMN_LABEL**
[`lang_map`](../../data-types.md#lang-ids) | Header in the list for all Bitrix24 languages ||
|| **LIST_FILTER_LABEL**
[`lang_map`](../../data-types.md#lang-ids) | Filter label in the list for all Bitrix24 languages ||
|| **ERROR_MESSAGE**
[`lang_map`](../../data-types.md#lang-ids) | Error message for all Bitrix24 languages ||
|| **HELP_MESSAGE**
[`lang_map`](../../data-types.md#lang-ids) | Help for all Bitrix24 languages ||
|| **USER_TYPE_OWNER**
[`string`](../../../data-types.md) | `CLIENT_ID` of the application that serves this field type. Returned if the field type is custom. If the application is not found, `null` ||
|#


#### uf_enum_element Type {#uf_enum_element}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../../data-types.md) | Identifier of the list element ||
|| **VALUE**
[`string`](../../../data-types.md) | Value of the list element ||
|| **SORT**
[`string`](../../../data-types.md) | Sorting index ||
|| **DEF**
[`char`](../../../data-types.md) | Indicates whether the list element is the default value. Possible values:
- `Y` — yes
- `N` — no
||
|#

The method does not return the list item's external code `XML_ID`. [crm.contact.userfield.list](./crm-contact-userfield-list.md) returns it.

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
|| `400` | Empty value | `Access denied.` | The user does not have read permission for contacts, or the field is not linked to contacts ||
|| `400` | Empty value | `ID is not defined or invalid.` | The `id` parameter is missing, is not numeric, or is less than or equal to zero ||
|| `400` | `ERROR_NOT_FOUND` | `The entity with ID 'id' is not found.` | The custom field with the provided `id` was not found ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-contact-userfield-add.md)
- [{#T}](./crm-contact-userfield-update.md)
- [{#T}](./crm-contact-userfield-list.md)
- [{#T}](./crm-contact-userfield-delete.md)
