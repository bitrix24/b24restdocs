# Get the list of call participants crm.calllist.items.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with read access permission to CRM entities

The method `crm.calllist.items.get` returns a list of participants, contacts, or companies, along with the call status.

## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **LIST_ID***
[`integer`](../../data-types.md) | Identifier of the call list, which can be obtained using the methods [crm.calllist.add](./crm-calllist-add.md) and [crm.calllist.list](./crm-calllist-list.md) ||
|| **FILTER**
[`object`](../../data-types.md) | Filter by the call status of the item: `{ STATUS: "status_code" }`. 
You can get the status code values using the method [crm.calllist.statuslist](./crm-calllist-statuslist.md)||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -d '{"LIST_ID":13,"FILTER":{"STATUS":"IN_WORK"}}' \
         https://**your_bitrix24**/rest/**user_id**/**webhook**/crm.calllist.items.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -d '{"LIST_ID":13,"FILTER":{"STATUS":"IN_WORK"},"auth":"**put_access_token_here**"}' \
         https://**your_bitrix24**/rest/crm.calllist.items.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each item returned in result[]
    type CallListItem = {
      ID: number,
      STATUS: string,
      ENTITY_TYPE: number,
    }

    try {
      const response = await $b24.actions.v2.call.make<CallListItem[]>({
        method: 'crm.calllist.items.get',
        params: {
          LIST_ID: 13,
          FILTER: {
            STATUS: 'IN_WORK',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Call list items count:', result.length, result)
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
      async function getCallListItems() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.calllist.items.get',
            params: {
              LIST_ID: 13,
              FILTER: {
                STATUS: 'IN_WORK',
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
          console.info('Call list items count:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCallListItems)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.calllist.items.get',
                [
                    'LIST_ID' => 13,
                    'FILTER' => [
                        'STATUS' => 'IN_WORK'
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting call list items: ' . $e->getMessage();
    }
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.calllist.items.get(
            list_id=13,
            filter={
                "STATUS": "IN_WORK",
            },
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

- BX24.js

    ```js
    BX24.callMethod(
        "crm.calllist.items.get",
        {
            LIST_ID: 13,
            FILTER: {
                STATUS: "IN_WORK"
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.dir(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.calllist.items.get',
        [ 'LIST_ID' => 13, 'FILTER' => [ 'STATUS' => 'IN_WORK' ] ]
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
    "result": [
        {
            "ID": 9,
            "STATUS": "IN_WORK",
            "ENTITY_TYPE": 3
        },
        {
            "ID": 17,
            "STATUS": "IN_WORK",
            "ENTITY_TYPE": 3
        },
        {
            "ID": 19,
            "STATUS": "IN_WORK",
            "ENTITY_TYPE": 3
        }
    ],
    "time": {
        "start": 1752475017.529502,
        "finish": 1752475017.588903,
        "duration": 0.05940103530883789,
        "processing": 0.010075092315673828,
        "date_start": "2025-07-14T09:36:57+02:00",
        "date_finish": "2025-07-14T09:36:57+02:00",
        "operating_reset_at": 1752475617,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array of items with call statuses and object types ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "Incorrect list id",
    "error_description": "An incorrect list identifier was provided."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `400` | `Incorrect list id` | Incorrect call list identifier ||
|| `400` | `Incorrect status` | Incorrect call status ||
|| `403` | `Access denied` | No access to list items ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-calllist-add.md)
- [{#T}](./crm-calllist-get.md)
- [{#T}](./crm-calllist-list.md)
- [{#T}](./crm-calllist-statuslist.md)
- [{#T}](./crm-calllist-update.md)