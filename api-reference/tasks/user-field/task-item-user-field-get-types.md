# Get a List of Available Data Types for task.item.userfield.gettypes

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.item.userfield.gettypes` retrieves the available types of custom fields.

Only the following types are supported for tasks:
- `string` — string
- `double` — number
- `datetime` — date and time
- `boolean` — yes/no

{% note warning " " %}

The method returns more types in the response, but only `string`, `double`, `datetime`, and `boolean` are supported in task custom fields.

{% endnote %}

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.userfield.gettypes
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/task.item.userfield.gettypes
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each item returned in result
    type UserFieldTypeItem = {
      ID: string
      title: string
    }

    try {
      const response = await $b24.actions.v2.call.make<UserFieldTypeItem[]>({
        method: 'task.item.userfield.gettypes',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Available user field types:', result.map(t => t.ID))
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
      async function getUserFieldTypes() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.item.userfield.gettypes',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Available user field types:', result.map(t => t.ID))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getUserFieldTypes)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.item.userfield.gettypes',
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
        'task.item.userfield.gettypes',
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
        'task.item.userfield.gettypes',
        []
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

The method returns more types in the response, but only `string`, `double`, `datetime`, and `boolean` are supported in task custom fields.

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
            "title": "Enumeration"
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
            "title": "URL"
        },
        {
            "ID": "address",
            "title": "Google Map Address"
        },
        {
            "ID": "file",
            "title": "File"
        },
        {
            "ID": "employee",
            "title": "User Binding"
        },
        {
            "ID": "crm_status",
            "title": "CRM Reference Binding"
        },
        {
            "ID": "iblock_section",
            "title": "Information Block Section Binding"
        },
        {
            "ID": "iblock_element",
            "title": "Information Block Element Binding"
        },
        {
            "ID": "crm",
            "title": "CRM Element Binding"
        }
    ],
    "total": 0,
    "time": {
        "start": 1772702743,
        "finish": 1772702743.192765,
        "duration": 0.1927649974822998,
        "processing": 0,
        "date_start": "2026-03-05T12:25:43+01:00",
        "date_finish": "2026-03-05T12:25:43+01:00",
        "operating_reset_at": 1772703343,
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
|| **total**
[`integer`](../../data-types.md) | Currently returns `0` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Code of the custom field type ||
|| **title**
[`string`](../../data-types.md) | Name of the custom field type ||
|#

## Error Handling

{% include notitle [error handling](../../../_includes/error-info.md) %}

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-item-user-field-add.md)
- [{#T}](./task-item-user-field-update.md)
- [{#T}](./task-item-user-field-get.md)
- [{#T}](./task-item-user-field-get-list.md)
- [{#T}](./task-item-user-field-delete.md)
- [{#T}](./task-item-user-field-get-fields.md)