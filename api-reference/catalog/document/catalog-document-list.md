# Get List of Inventory Documents catalog.document.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "View and select warehouses" permission

The method `catalog.document.list` returns a list of inventory documents with filtering, sorting, and pagination.

The response includes only documents of the types for which the user has the "View" permission, and only with warehouses available to the user. The method does not return reservation cancellation documents, type `U`.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | Array of document fields to return. Available fields: `commentary`, `createdBy`, `currency`, `dateCreate`, `dateDocument`, `dateModify`, `dateStatus`, `docNumber`, `docType`, `id`, `modifiedBy`, `responsibleId`, `siteId`, `status`, `statusBy`, `title`, `total`. Field descriptions are in the [catalog_document](../data-types.md#catalog_document) object.

If the array is not passed or is empty, the method returns all fields. The method skips unknown fields without an error. Write field names in camelCase: if `select` contains no known field, for example `["ID", "TITLE"]`, the method returns all fields
||
|| **filter**
[`object`](../../data-types.md) | Object for filtering documents in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`.

Possible values for `field` are the fields from the `select` list. For example, `{"docType": "S", "status": "N"}` returns draft stock adjustment documents.

An additional prefix can be set for the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol must be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` symbol must be included in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

An array without a prefix works as IN: `{"id": [359, 369]}` returns both documents.

The method skips unknown fields and uppercase names, for example `ID`, without an error and returns documents ignoring such a condition. Check field names against the list above
|| 
|| **order**
[`object`](../../data-types.md) | Object for sorting documents in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` are the fields from the `select` list.

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order

The direction is case-insensitive: `DESC` and `desc` work the same way. The method skips unknown fields in `order` without an error. By default, documents are sorted by `id` in ascending order
||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to manage pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results, pass the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the number of the required page.

If there is a next page, the response contains the `next` key with the `start` value for it. If `start` is greater than the number of documents found, the method returns an empty `documents` array
||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

The example retrieves draft stock adjustment documents whose name contains "Surplus stock adjustment", starting with the newest.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","docType","docNumber","title","status","dateDocument","total"],"filter":{"docType":"S","status":"N","%title":"Surplus stock adjustment"},"order":{"id":"desc"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","docType","docNumber","title","status","dateDocument","total"],"filter":{"docType":"S","status":"N","%title":"Surplus stock adjustment"},"order":{"id":"desc"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of one document returned in result.documents (only the selected fields)
    type DocumentListItem = {
      id: number
      docType: string
      docNumber: string | null
      title: string | null
      status: string
      dateDocument: ISODate | null
      total: number | null
    }

    try {
      // catalog.document.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<{ documents: DocumentListItem[] }>({
        method: 'catalog.document.list',
        params: {
          select: ['id', 'docType', 'docNumber', 'title', 'status', 'dateDocument', 'total'],
          filter: {
            docType: 'S',
            status: 'N',
            '%title': 'Surplus stock adjustment',
          },
          order: {
            id: 'desc',
          },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Documents on page:', result.documents.length)
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
      async function listDocuments() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.document.list',
            params: {
              select: ['id', 'docType', 'docNumber', 'title', 'status', 'dateDocument', 'total'],
              filter: {
                docType: 'S',
                status: 'N',
                '%title': 'Surplus stock adjustment',
              },
              order: {
                id: 'desc',
              },
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
          console.info('Documents on page:', result.documents.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listDocuments)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.list(
            select=["id", "docType", "docNumber", "title", "status", "dateDocument", "total"],
            filter={
                "docType": "S",
                "status": "N",
                "%title": "Surplus stock adjustment",
            },
            order={
                "id": "desc",
            },
            start=0,
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
                'catalog.document.list',
                [
                    'select' => ['id', 'docType', 'docNumber', 'title', 'status', 'dateDocument', 'total'],
                    'filter' => [
                        'docType' => 'S',
                        'status'  => 'N',
                        '%title'  => 'Surplus stock adjustment',
                    ],
                    'order'  => [
                        'id' => 'desc',
                    ],
                    'start'  => 0,
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
        'catalog.document.list',
        {
            select: ['id', 'docType', 'docNumber', 'title', 'status', 'dateDocument', 'total'],
            filter: {
                docType: 'S',
                status: 'N',
                '%title': 'Surplus stock adjustment',
            },
            order: {
                id: 'desc',
            },
            start: 0,
        },
        function(result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.document.list',
        [
            'select' => ['id', 'docType', 'docNumber', 'title', 'status', 'dateDocument', 'total'],
            'filter' => [
                'docType' => 'S',
                'status' => 'N',
                '%title' => 'Surplus stock adjustment',
            ],
            'order' => [
                'id' => 'desc',
            ],
            'start' => 0,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.document.list", b24.Params{
    	"select": []string{"id", "docType", "docNumber", "title", "status", "dateDocument", "total"},
    	"filter": b24.Params{
    		"docType": "S",
    		"status":  "N",
    		"%title":  "Surplus stock adjustment",
    	},
    	"order": b24.Params{
    		"id": "desc",
    	},
    	"start": 0,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.document.list: %w", err)
    }

    // The method wraps the response in an object with the "documents" key.
    raw, ok := b24.Unwrap(res.Result, "documents")
    if !ok {
    	return fmt.Errorf("no documents key in the response")
    }

    // Optional fields can be null, so pointers are used for them.
    var items []struct {
    	ID           b24.ID   `json:"id"`
    	DocType      string   `json:"docType"`
    	DocNumber    *string  `json:"docNumber"`
    	Title        *string  `json:"title"`
    	Status       string   `json:"status"`
    	DateDocument *string  `json:"dateDocument"`
    	Total        *float64 `json:"total"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.Status)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "documents": [
            {
                "dateDocument": "2026-11-06T10:00:00+03:00",
                "docNumber": "OP-00042",
                "docType": "S",
                "id": 381,
                "status": "N",
                "title": "Surplus stock adjustment",
                "total": null
            }
        ]
    },
    "total": 1,
    "time": {
        "start": 1793883564,
        "finish": 1793883564.287555,
        "duration": 0.2875549793243408,
        "processing": 0,
        "date_start": "2026-11-05T15:59:24+03:00",
        "date_finish": "2026-11-05T15:59:24+03:00",
        "operating_reset_at": 1793884140,
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
|| **result.documents**
[`catalog_document[]`](../data-types.md#catalog_document) | Array of documents. Each document has only the fields from `select`, empty fields come with the value `null` ||
|| **next**
[`integer`](../../data-types.md) | The `start` value for the next page. The key is absent if the page is the last one ||
|| **total**
[`integer`](../../data-types.md) | Total number of documents found ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "100",
    "error_description": "Invalid order \"BAD\""
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Insufficient permissions to save the inventory object. | The user has neither the "View product catalog" permission nor the "View and select warehouses" permission ||
|| `400` | `100` | Invalid order "BAD" | `order` contains a direction other than `asc` and `desc`. The text shows the direction in uppercase ||
|| `400` | `100` | Invalid value {abc} to match with parameter {filter}. Should be value of type array. | `filter` is not an object. The same error comes with `{select}` or `{order}` if they are not an array or an object ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-add.md)
- [{#T}](./catalog-document-update.md)
- [{#T}](./catalog-document-delete.md)
- [{#T}](./catalog-document-conduct.md)
- [{#T}](./catalog-document-get-fields.md)
- [{#T}](./document-element/catalog-document-element-list.md)
