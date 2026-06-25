# Add Field to Duplicate Search crm.duplicate.volatileType.register

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `crm.duplicate.volatileType.register` adds a field to the duplicate search functionality in leads, contacts, or companies.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId*** 
[`integer`](../../../data-types.md) | Identifier for the object type. Possible values:
- `1` — [lead](../../leads/index.md)
- `3` — [contact](../../contacts/index.md)
- `4` — [company](../../companies/index.md) ||
|| **fieldCode*** 
[`string`](../../../data-types.md) | The code of the field to be added to the duplicate search. For example, `TITLE`, `RQ.RU.NAME`, `UF_CRM_1750854801`. You can get a list of available fields using the method [crm.duplicate.volatileType.fields](./crm-duplicate-volatile-type-fields.md) ||
|#

### Method Operation Features

A total of 7 custom fields can be registered for duplicate searches. For example, if you have already added 3 fields for contacts and 4 fields for companies, attempting to add another field for any object type will result in the error `MAX_TYPES_COUNT_EXCEEDED`.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"entityTypeId":1,"fieldCode":"TITLE"}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.duplicate.volatileType.register
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1,"fieldCode":"TITLE","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.duplicate.volatileType.register
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type RegisterVolatileTypeResult = {
      id: number
    }

    try {
      const response = await $b24.actions.v2.call.make<RegisterVolatileTypeResult>({
        method: 'crm.duplicate.volatileType.register',
        params: {
          entityTypeId: 1,
          fieldCode: 'TITLE',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Registered field with id:', result.id)
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
      async function registerVolatileType() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.duplicate.volatileType.register',
            params: {
              entityTypeId: 1,
              fieldCode: 'TITLE',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Registered field with id:', result.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', registerVolatileType)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.duplicate.volatileType.register',
                [
                    'entityTypeId' => 1,
                    'fieldCode'    => 'TITLE',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error registering volatile type: ' . $e->getMessage();
    }
    ```

- Python

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.duplicate.volatile_type.register(
            entity_type_id=1,
            field_code="TITLE",
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
        "crm.duplicate.volatileType.register",
        {
            entityTypeId: 1,
            fieldCode: "TITLE"
        },
        function(result) {
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
        'crm.duplicate.volatileType.register',
        [
            'entityTypeId' => 1,
            'fieldCode' => 'TITLE'
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
    "result": {
        "id": 3355
    },
    "time": {
        "start": 1750934251.736599,
        "finish": 1750934252.028757,
        "duration": 0.2921581268310547,
        "processing": 0.24904417991638184,
        "date_start": "2025-06-26T13:37:31+03:00",
        "date_finish": "2025-06-26T13:37:32+03:00",
        "operating_reset_at": 1750934851,
        "operating": 0.24902796745300293
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`integer`](../../../data-types.md) | Identifier for the record of the field added to the duplicate search ||
|| **time**[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "Field not found",
    "error_description": "Specified field not found."
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `400` | `Field not found` | The specified field was not found ||
|| `400` | `MAX_TYPES_COUNT_EXCEEDED` | The maximum number of custom field types in the duplicate search has been exceeded ||
|#

{% include [System errors](./../../../../_includes/system-errors.md) %}

## Continue Learning

- [crm.duplicate.volatileType.fields](./crm-duplicate-volatile-type-fields.md)
- [crm.duplicate.volatileType.list](./crm-duplicate-volatile-type-list.md)
- [crm.duplicate.volatileType.unregister](./crm-duplicate-volatile-type-unregister.md) 