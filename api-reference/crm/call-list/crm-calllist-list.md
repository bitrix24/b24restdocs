# Get the list of call lists crm.calllist.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: user with read access permission for CRM elements

The method `crm.calllist.list` returns a list of call list activities.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **SELECT**
[`array`](../../data-types.md) | Array of fields to retrieve. By default, all fields are returned.
List of fields to retrieve:
- `ID`,
- `DATE_CREATE`,
- `CREATED_BY_ID`,
- `WEBFORM_ID`,
- `ENTITY_TYPE_ID` ||
|| **FILTER**
[`object`](../../data-types.md) | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

- `field_n` — name of the field by which the call list will be filtered
- `value_n` — filter value

List of fields for filtering:
- `ID`,
- `CREATED_BY_ID`,
- `WEBFORM_ID`,
- `ENTITY_TYPE_ID`.
  
An additional prefix can be assigned to the field to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to,
- `>` — greater than,
- `<=` — less than or equal to,
- `<` — less than,
- `@` — IN, an array is passed as the value,
- `!@`— NOT IN, an array is passed as the value,
- `=` — equal, exact match, used by default,
- `!=` - not equal,
- `!` — not equal ||
|| **ORDER**
[`object`](../../data-types.md) | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

- `field_n` — name of the field by which the call list will be sorted
- `value_n` — value of type `string`, equal to:
    - `ASC` — ascending sort
    - `DESC` — descending sort ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"SELECT":["ID","CREATED_BY_ID"],"FILTER":{"ENTITY_TYPE_ID":3},"ORDER":{"ID":"DESC"}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.calllist.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"SELECT":["ID","CREATED_BY_ID"],"FILTER":{"ENTITY_TYPE_ID":3},"ORDER":{"ID":"DESC"},"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/crm.calllist.list
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
      ID: string
      CREATED_BY_ID: string
    }

    // crm.calllist.list returns a single page (max 50 records). For the whole result set
    // use a list helper: $b24.actions.v2.callList.make() returns every record as one
    // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
    // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
    // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
    try {
      const response = await $b24.actions.v2.call.make<CallListItem[]>({
        method: 'crm.calllist.list',
        params: {
          SELECT: ['ID', 'CREATED_BY_ID'],
          FILTER: { ENTITY_TYPE_ID: 3 },
          ORDER: { ID: 'DESC' },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Call lists fetched:', result.length, result)
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
      async function fetchCallLists() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.calllist.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'crm.calllist.list',
            params: {
              SELECT: ['ID', 'CREATED_BY_ID'],
              FILTER: { ENTITY_TYPE_ID: 3 },
              ORDER: { ID: 'DESC' },
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Call lists fetched:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchCallLists)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.calllist.list',
                [
                    'SELECT' => ['ID', 'CREATED_BY_ID'],
                    'FILTER' => ['ENTITY_TYPE_ID' => 3],
                    'ORDER'  => ['ID' => 'DESC'],
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
        echo 'Error fetching call list: ' . $e->getMessage();
    }
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.calllist.list(
            select=["ID", "CREATED_BY_ID"],
            filter={"ENTITY_TYPE_ID": 3},
            order={"ID": "DESC"},
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

    Example `as_list`

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.calllist.list(
            select=["ID", "CREATED_BY_ID"],
            filter={"ENTITY_TYPE_ID": 3},
            order={"ID": "DESC"},
        ).as_list().response
        result = bitrix_response.result
        for item in result:
            print(item)
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
        "crm.calllist.list",
        {
            SELECT: ["ID", "CREATED_BY_ID"],
            FILTER: { "ENTITY_TYPE_ID": 3 },
            ORDER: { "ID": "DESC" }
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
        'crm.calllist.list',
        [
            'SELECT' => ['ID', 'CREATED_BY_ID'],
            'FILTER' => ['ENTITY_TYPE_ID' => 3],
            'ORDER' => ['ID' => 'DESC']
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
    "result": [
        {
            "ID": "13",
            "CREATED_BY_ID": "29"
        },
        {
            "ID": "9",
            "CREATED_BY_ID": "1"
        },
        {
            "ID": "3",
            "CREATED_BY_ID": "131"
        },
        {
            "ID": "1",
            "CREATED_BY_ID": "131"
        }
    ],
    "time": {
        "start": 1752475786.965766,
        "finish": 1752475787.035008,
        "duration": 0.06924200057983398,
        "processing": 0.016666889190673828,
        "date_start": "2025-07-14T09:49:46+02:00",
        "date_finish": "2025-07-14T09:49:47+02:00",
        "operating_reset_at": 1752476387,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Root element of the response. Contains an array of objects with information about call lists. 

The structure of fields depends on the `select` parameter ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "Invalid parameters.",
    "error_description": "Invalid parameters were provided."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `400` | `Invalid parameters` | Invalid parameters were provided ||
|| `100` | `Unknown field definition "TITLE"` | Unknown parameter "Field name" ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-calllist-add.md)
- [{#T}](./crm-calllist-get.md)
- [{#T}](./crm-calllist-items-get.md)
- [{#T}](./crm-calllist-statuslist.md)
- [{#T}](./crm-calllist-update.md)

