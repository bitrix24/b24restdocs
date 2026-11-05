# Get List of Products in Inventory Documents catalog.document.element.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "View and select warehouses" permission

The method `catalog.document.element.list` returns product items of inventory documents with filtering, sorting, and pagination.

The response includes items only of documents whose type the user has the "View" permission for, and only of warehouses available under the "View and select warehouses" permission. Without the "View purchase price" permission, the `purchasingPrice` field is returned as `null`.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | Array of item fields to return. Available fields: `id`, `docId`, `elementId`, `storeFrom`, `storeTo`, `amount`, `purchasingPrice`. The fields are described in the [catalog_document_element](../../data-types.md#catalog_document_element) object.

If the array is not passed or is empty, the method returns all fields. The method skips unknown fields without an error. Write field names in camelCase: if `select` contains no known field, for example `["ID", "AMOUNT"]`, the method returns all fields ||
|| **filter**
[`object`](../../../data-types.md) | Object for filtering items in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`.

Possible values for `field` are the fields from the list in `select`. For example, `{"docId": 371}` returns the items of document `371`.

An additional prefix can be set for the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

An array without a prefix works as IN: `{"id": [71, 101]}` returns both items. A `null` value without a prefix finds items with an empty field, for example `{"amount": null}`.

The `<` and `<=` conditions can also find items with an empty field. The substring search prefixes `%`, `=%`, and `!%` do not apply to item fields: all fields are numeric.

The method skips unknown fields and uppercase names, for example `AMOUNT`, without an error and returns items without applying such a condition. Check field names against the list above ||
|| **order**
[`object`](../../../data-types.md) | Object for sorting items in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` are the fields from the list in `select`.

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order

The direction is case-insensitive: `DESC` and `desc` work the same way. The method skips unknown fields in `order` without an error ||
|| **start**
[`integer`](../../../data-types.md) | This parameter is used for managing pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the desired page number.

If `start` is greater than the number of found items, the method returns an empty `documentElements` array ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","docId","elementId","amount","storeFrom","storeTo"],"filter":{"docId":371},"order":{"id":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.element.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","docId","elementId","amount","storeFrom","storeTo"],"filter":{"docId":371},"order":{"id":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.element.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of one position (only the selected fields); empty fields come as null
    type DocumentElement = {
      id: number
      docId: number
      elementId: number | null
      amount: number | null
      storeFrom: number | null
      storeTo: number | null
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DocumentElementListResult = {
      documentElements: DocumentElement[]
    }

    try {
      // catalog.document.element.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<DocumentElementListResult>({
        method: 'catalog.document.element.list',
        params: {
          select: [
            'id',
            'docId',
            'elementId',
            'amount',
            'storeFrom',
            'storeTo',
          ],
          filter: {
            docId: 371,
          },
          order: {
            id: 'ASC',
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
        console.info(result.documentElements.length, result.documentElements)
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
      async function listDocumentElements() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // catalog.document.element.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'catalog.document.element.list',
            params: {
              select: [
                'id',
                'docId',
                'elementId',
                'amount',
                'storeFrom',
                'storeTo',
              ],
              filter: {
                docId: 371,
              },
              order: {
                id: 'ASC',
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
          console.info(result.documentElements.length, result.documentElements)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listDocumentElements)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.element.list(
            select=[
                "id",
                "docId",
                "elementId",
                "amount",
                "storeFrom",
                "storeTo",
            ],
            filter={
                "docId": 371,
            },
            order={
                "id": "ASC",
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
                'catalog.document.element.list',
                [
                    'select' => [
                        'id',
                        'docId',
                        'elementId',
                        'amount',
                        'storeFrom',
                        'storeTo',
                        'purchasingPrice'
                    ],
                    'filter' => [
                        'docId' => 371
                    ],
                    'order' => [
                        'id' => 'ASC'
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching document elements: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.document.element.list',
        {
            select: [
                'id',
                'docId',
                'elementId',
                'amount',
                'storeFrom',
                'storeTo'
            ],
            filter: {
                docId: 371
            },
            order: {
                id: 'ASC'
            }
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
        'catalog.document.element.list',
        [
            'select' => [
                'id',
                'docId',
                'elementId',
                'amount',
                'storeFrom',
                'storeTo'
            ],
            'filter' => [
                'docId' => 371
            ],
            'order' => [
                'id' => 'ASC'
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.document.element.list", b24.Params{
    	"select": []string{"id", "docId", "elementId", "amount", "storeFrom", "storeTo"},
    	"filter": b24.Params{
    		"docId": 371,
    	},
    	"order": b24.Params{
    		"id": "ASC",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.document.element.list: %w", err)
    }

    // The method wraps the response in an object with the "documentElements" key.
    raw, ok := b24.Unwrap(res.Result, "documentElements")
    if !ok {
    	return fmt.Errorf("no documentElements key in the response")
    }

    // Fields that are not filled in are returned as null, so they use pointers.
    var items []struct {
    	Amount    *float64 `json:"amount"`
    	DocID     b24.ID   `json:"docId"`
    	ElementID *int     `json:"elementId"`
    	ID        b24.ID   `json:"id"`
    	StoreFrom *int     `json:"storeFrom"`
    	StoreTo   *int     `json:"storeTo"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.Amount)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "documentElements": [
            {
                "amount": 20,
                "docId": 371,
                "elementId": 6595,
                "id": 71,
                "storeFrom": null,
                "storeTo": 43
            },
            {
                "amount": 12,
                "docId": 371,
                "elementId": 6595,
                "id": 101,
                "storeFrom": null,
                "storeTo": 43
            }
        ]
    },
    "total": 2,
    "time": {
        "start": 1793884005,
        "finish": 1793884005.329118,
        "duration": 0.329118013381958,
        "processing": 0,
        "date_start": "2026-11-05T16:06:45+03:00",
        "date_finish": "2026-11-05T16:06:45+03:00",
        "operating_reset_at": 1793884560,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **result.documentElements**
[`catalog_document_element[]`](../../data-types.md#catalog_document_element) | Array of items. Each item contains only the fields from `select`; fields that are not filled in are returned as `null` ||
|| **next**
[`integer`](../../../data-types.md) | The `start` value for retrieving the next page. Returned if more than 50 items are found ||
|| **total**
[`integer`](../../../data-types.md) | Total number of found items ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "100",
    "error_description": "Invalid order \"BAD\""
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_DOCUMENT_RIGHTS` | Access denied | The user has neither the "View product catalog" permission nor the "View and select warehouses" permission ||
|| `400` | `100` | Invalid order "BAD" | `order` contains a direction other than `asc` and `desc`. The message shows the direction in uppercase ||
|| `400` | `100` | Invalid value {abc} to match with parameter {filter}. Should be value of type array. | `filter` is not an object. The same error occurs with `{select}` or `{order}` if they are neither an array nor an object ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-element-add.md)
- [{#T}](./catalog-document-element-update.md)
- [{#T}](./catalog-document-element-delete.md)
- [{#T}](./catalog-document-element-get-fields.md)
