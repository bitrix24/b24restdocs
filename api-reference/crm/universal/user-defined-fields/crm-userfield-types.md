# Get a list of custom field types crm.userfield.types

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.userfield.types` retrieves the available types of custom fields.

The types returned are:
- `string` — string
- `integer` — integer
- `double` — number
- `boolean` — yes/no
- `datetime` — date and time
- `enumeration` — list
- `iblock_section` — binding to information block sections
- `iblock_element` — binding to information block elements
- `employee` — binding to an employee
- `crm_status` — binding to the CRM directory
- `crm` — binding to CRM entities
- `address` — Google Maps address
- `money` — money
- `url` — link

The method also returns [custom types](./userfield-type.md) registered by the current application.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.userfield.types
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.userfield.types
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each item returned in result[]
    type UserfieldTypeItem = {
      ID: string
      title: string
    }

    try {
      const response = await $b24.actions.v2.call.make<UserfieldTypeItem[]>({
        method: 'crm.userfield.types',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Field types:', result.length, result.map(t => t.ID))
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
      async function getUserfieldTypes() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.userfield.types',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Field types:', result.length, result.map(t => t.ID))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getUserfieldTypes)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.userfield.types',
                []
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
        "crm.userfield.types",
        {},
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.userfield.types',
        []
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "ID": "string",
            "title": "String"
        },
        {
            "ID": "integer",
            "title": "Integer"
        },
        {
            "ID": "double",
            "title": "Number"
        },
        {
            "ID": "boolean",
            "title": "Yes/No"
        },
        {
            "ID": "enumeration",
            "title": "List"
        },
        {
            "ID": "datetime",
            "title": "Date/Time"
        },
        {
            "ID": "date",
            "title": "Date"
        },
        {
            "ID": "money",
            "title": "Money"
        },
        {
            "ID": "url",
            "title": "Link"
        },
        {
            "ID": "address",
            "title": "Google Maps Address"
        },
        {
            "ID": "file",
            "title": "File"
        },
        {
            "ID": "employee",
            "title": "Binding to User"
        },
        {
            "ID": "crm_status",
            "title": "Binding to CRM Directories"
        },
        {
            "ID": "iblock_section",
            "title": "Binding to Information Block Sections"
        },
        {
            "ID": "iblock_element",
            "title": "Binding to Information Block Elements"
        },
        {
            "ID": "crm",
            "title": "Binding to CRM Entities"
        }
    ],
    "time": {
        "start": 1773992560,
        "finish": 1773992560.044522,
        "duration": 0.04452204704284668,
        "processing": 0,
        "date_start": "2026-03-20T10:42:40+02:00",
        "date_finish": "2026-03-20T10:42:40+02:00",
        "operating_reset_at": 1773993160,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array of objects with supported types [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Code of the custom field type ||
|| **title**
[`string`](../../data-types.md) | Name of the custom field type ||
|#

## Error Handling

{% include notitle [error handling](../../../../_includes/error-info.md) %}

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-userfield-fields.md)
- [{#T}](./crm-userfield-settings-fields.md)
- [{#T}](./crm-userfield-enumeration-fields.md)