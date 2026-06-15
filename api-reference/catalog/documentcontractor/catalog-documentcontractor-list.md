# Get a List of Vendor Bindings to Documents catalog.documentcontractor.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the following access permissions:
> — "View" on the document type "Incoming",
> — "View" on the Inventory Accounting section,
> — "View" on the Product Catalog    

The method `catalog.documentcontractor.list` returns a list of vendor bindings to inventory accounting documents. By default, filters are applied to the request that limit the selection based on the current user's permissions.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|  
|| **Name**  
`type` | **Description** ||  
|| **select**  
[`array`](../../data-types.md) | An array containing the list of fields from [catalog_documentcontractor](../data-types.md#catalog_documentcontractor) that need to be selected.

If the array is not provided or an empty array is passed, all available fields of the document will be selected. ||  
|| **filter**  
[`object`](../../data-types.md) | An object for filtering the selected bindings in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [catalog_documentcontractor](../data-types.md#catalog_documentcontractor) object.

An additional prefix can be specified for the key to clarify the filter's behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `=` — exact match
- `!=`, `!` — not equal
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol must be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value. The search is conducted from both sides.
- `!=%` — NOT LIKE, substring search. The `%` symbol must be included in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is absent in any position
- `=` — equal, exact match, used by default
- `!=` — not equal
- `!` — not equal
||  
|| **order**  
[`object`](../../data-types.md) | An object for sorting the selected documents in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [catalog_documentcontractor](../data-types.md#catalog_documentcontractor) object.

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order 

By default, results are sorted in ascending order by `id`. ||  
|| **start**  
[`integer`](../../data-types.md) | This parameter is used to manage pagination.

The page size is 50 records.

To select the second page of results, pass the value `50`. To select the third page of results, pass the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number. 
Or pass the value from the `next` key in the response. ||  
|#  

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","documentId"],"filter":{"documentId":7},"order":{"id":"ASC"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.documentcontractor.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","documentId"],"filter":{"documentId":7},"order":{"id":"ASC"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.documentcontractor.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DocumentContractorResult = {
      documentContractor: {
        id: number
        documentId: number
      }[]
    }

    try {
      // catalog.documentcontractor.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<DocumentContractorResult>({
        method: 'catalog.documentcontractor.list',
        params: {
          select: ['id', 'documentId'],
          filter: { documentId: 7 },
          order: { id: 'ASC' },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.documentContractor.length, result.documentContractor)
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
      async function fetchDocumentContractorList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // catalog.documentcontractor.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'catalog.documentcontractor.list',
            params: {
              select: ['id', 'documentId'],
              filter: { documentId: 7 },
              order: { id: 'ASC' },
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
          console.info(result.documentContractor.length, result.documentContractor)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchDocumentContractorList)
    </script>
    ```

- PHP

    ```php  
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.documentcontractor.list',
                [
                    'select' => ["id", "documentId"],
                    'filter' => ['documentId' => 7],
                    'order' => ['id' => 'ASC'],
                    'start' => 0
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.documentcontractor.list',
        {
            select: ["id", "documentId"],
            filter: { "documentId": 7 },
            order: { "id": "ASC" }
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result.data());

                if (result.more())
                {
                    result.next();
                }
            }
        }
    );
    ```	

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.documentcontractor.list',
        [
            'select' => ["id", "documentId"],
            'filter' => ['documentId' => 7],
            'order' => ['id' => 'ASC'],
            'start' => 0
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": {
        "documentContractor": [
        {
            "documentId": 7,
            "id": 5
        }
        ]
    },
    "total": 1,
    "time": {
        "start": 1766146580,
        "finish": 1766146580.866608,
        "duration": 0.8666079044342041,
        "processing": 0,
        "date_start": "2025-12-19T15:16:20+01:00",
        "date_finish": "2025-12-19T15:16:20+01:00",
        "operating_reset_at": 1766147180,
        "operating": 0
    }
}
```

### Returned Data

#|  
|| **Name**  
`type` | **Description** ||  
|| **result**  
[`object`](../../data-types.md) | The root element of the response ||  
|| **documentContractor**  
[`catalog_documentContractor[]`](../data-types.md#catalog_documentContractor) | A list of contractor bindings; the response structure depends on the `select` parameter ||  
|| **next**  
[`integer`](../../data-types.md) | Offset pointer for the next page. Pass this value to the `start` parameter to retrieve the next 50 records ||  
|| **total**  
[`integer`](../../data-types.md) | The total number of documents ||  
|| **time**  
[`time`](../../data-types.md#time) | Information about the request execution time ||  
|#  

## Error Handling

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

HTTP Code: **400**

```json
{
    "error": "0",
    "error_description": "Access denied"
}
```

### Possible Error Codes  

#|  
|| **Code** | **Description** | **Value** ||  
|| `0` | Access denied | Insufficient permissions to view the document or bindings ||  
|| `0` | Contractors should be provided by CRM | The CRM module is not active as a vendor for contractors ||  
|# 

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-documentcontractor-add.md)  
- [{#T}](./catalog-documentcontractor-delete.md)  
- [{#T}](./catalog-documentcontractor-get-fields.md)  