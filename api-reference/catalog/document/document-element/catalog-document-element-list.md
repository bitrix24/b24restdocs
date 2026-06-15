# Get a list of products in inventory documents catalog.document.element.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: users with the "View product catalog" access permission

The method `catalog.document.element.list` returns product positions associated with inventory documents. Records are automatically limited by available document types and the user's permissions on warehouses.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | An array of fields from [catalog_document_element](../../data-types.md#catalog_document_element) that need to be selected ||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering selected product records in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [catalog_document_element](../../data-types.md#catalog_document_element) object.

An additional prefix can be set for the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to,
- `>` — greater than,
- `<=` — less than or equal to,
- `<` — less than,
- `@` — IN, an array is passed as the value,
- `!@` — NOT IN, an array is passed as the value,
- `=` — equal, exact match, used by default,
- `!=` — not equal,
- `!` — not equal
||
|| **order**
[`object`](../../../data-types.md) | An object for sorting selected product records in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [catalog_document_element](../../data-types.md#catalog_document_element) object.

Possible values for `order`:
- `asc` — in ascending order,
- `desc` — in descending order 
||
|| **start**
[`integer`](../../../data-types.md) | This parameter is used for managing pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the desired page number ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","docId","elementId","amount","storeFrom","storeTo"],"filter":{"docId":64},"order":{"id":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.element.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","docId","elementId","amount","storeFrom","storeTo"],"filter":{"docId":64},"order":{"id":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.element.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentElement = {
      id: number
      docId: number
      elementId: number
      amount: number
      purchasingPrice: number
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
            docId: 64,
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
                docId: 64,
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
                        'docId' => 64
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
        $result->next();

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
                docId: 64
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

            result.next();
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
                'docId' => 64
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

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "documentElements": [
            {
                "amount": 5,
                "docId": 64,
                "elementId": 312,
                "id": 148,
                "purchasingPrice": 1180,
                "storeFrom": null,
                "storeTo": 2
            },
            {
                "amount": 12,
                "docId": 64,
                "elementId": 420,
                "id": 149,
                "purchasingPrice": 560,
                "storeFrom": null,
                "storeTo": 2
            }
        ]
    },
    "total": 2,
    "time": {
        "start": 1759482402.511337,
        "finish": 1759482402.642843,
        "duration": 0.13150620460510254,
        "processing": 0.02694106101989746,
        "date_start": "2025-11-02T12:26:42+02:00",
        "date_finish": "2025-11-02T12:26:42+02:00",
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
|| **documentElement**
[`catalog_document_element[]`](../../data-types.md#catalog_document_element) | Object with information about the document products, the response structure depends on the `select` parameter ||
|| **total**
[`integer`](../../../data-types.md) | Total number of records ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_DOCUMENT_RIGHTS",
    "error_description": "Access denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_DOCUMENT_RIGHTS` | Access denied | Insufficient rights to read ||
|| `0` |  | Other processing errors ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-document-element-get-fields.md)
- [{#T}](./catalog-document-element-add.md)
- [{#T}](./catalog-document-element-update.md)
- [{#T}](./catalog-document-element-delete.md)