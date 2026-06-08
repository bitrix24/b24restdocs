# Get Company Custom Field by ID crm.company.userfield.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: user with read access permission for companies

The method `crm.company.userfield.get` returns a company custom field by its ID.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the custom field associated with the company.

The identifier can be obtained using the methods [crm.company.userfield.add](./crm-company-userfield-add.md) or [crm.company.userfield.list](./crm-company-userfield-list.md) ||
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
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.userfield.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":399,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.company.userfield.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CrmCompanyUserfield = {
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
      const response = await $b24.actions.v2.call.make<CrmCompanyUserfield>({
        method: 'crm.company.userfield.get',
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
      async function getCompanyUserfield() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.company.userfield.get',
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

      document.addEventListener('DOMContentLoaded', getCompanyUserfield)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.company.userfield.get',
                [
                    'id' => 399,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting company user field: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.company.userfield.get',
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
        'crm.company.userfield.get',
        [
            'id' => 399
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
        "ID": "6997",
        "ENTITY_ID": "CRM_COMPANY",
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
            "ru": "",
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
            "ru": "",
            "sc": "",
            "tc": "",
            "th": "",
            "tr": "",
            "ua": "",
            "vn": ""
        },
        "LIST_FILTER_LABEL": {
            "ar": "Hello, World! Column (changed)",
            "br": "Hello, World! Column (changed)",
            "de": "Hello, World! Column (changed)",
            "en": "Hello, World! Column (changed)",
            "fr": "Hello, World! Column (changed)",
            "hi": "Hello, World! Column (changed)",
            "id": "Hello, World! Column (changed)",
            "in": "Hello, World! Column (changed)",
            "it": "Hello, World! Column (changed)",
            "ja": "Hello, World! Column (changed)",
            "kz": "Hello, World! Column (changed)",
            "la": "Hello, World! Column (changed)",
            "ms": "Hello, World! Column (changed)",
            "pl": "Hello, World! Column (changed)",
            "ru": "Hello, World! Column (changed)",
            "sc": "Hello, World! Column (changed)",
            "tc": "Hello, World! Column (changed)",
            "th": "Hello, World! Column (changed)",
            "tr": "Hello, World! Column (changed)",
            "ua": "Hello, World! Column (changed)",
            "vn": "Hello, World! Column (changed)"
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
            "ru": "",
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
            "ru": "",
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
[`object`](../../../data-types.md) | Root element of the response, contains information about the custom field. The final list of fields depends on the field type, detailed descriptions of the fields can be found in the method [crm.company.userfield.add](./crm-company-userfield-add.md) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
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
|| **Code** | **Description** | **Value** ||
|| `403` | Access denied | Occurs when:
- the user does not have read access permission for companies
- the user attempts to access a custom field not associated with companies ||
|| `400` | ID is not defined or invalid | The provided `id` is less than or equal to zero, or not provided at all ||
|| `ERROR_NOT_FOUND` | The entity with ID 'id' is not found | The custom field with the provided `id` was not found ||
|#
{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-company-userfield-add.md)
- [{#T}](./crm-company-userfield-update.md)
- [{#T}](./crm-company-userfield-list.md)
- [{#T}](./crm-company-userfield-delete.md)