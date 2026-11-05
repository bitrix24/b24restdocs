# Add Product to Inventory Document catalog.document.element.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Create and edit" permission for documents of this type and the "View and select warehouses" permission for the item warehouses

The method `catalog.document.element.add` adds a product item to an unprocessed inventory document.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Product item fields ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **docId***
[`catalog_document.id`](../../data-types.md#catalog_document) | Identifier of the inventory document. The document must be unprocessed, that is, have the `N` status.

The identifier can be retrieved using the [catalog.document.list](../catalog-document-list.md) method ||
|| **elementId**
[`catalog_product.id`](../../data-types.md#catalog_product) | Identifier of a simple product or variation.

The identifier can be retrieved using the [catalog.product.list](../../product/catalog-product-list.md) and [catalog.product.offer.list](../../product/offer/catalog-product-offer-list.md) methods.

The method does not check that the product exists. If the field is not passed, the method creates an item without a product ||
|| **storeFrom**
[`catalog_store.id`](../../data-types.md#catalog_store) | Identifier of the write-off warehouse. Filled in for transfer and write-off documents.

The identifier can be retrieved using the [catalog.store.list](../../store/catalog-store-list.md) method ||
|| **storeTo**
[`catalog_store.id`](../../data-types.md#catalog_store) | Identifier of the receipt warehouse. Filled in for receipt, stock adjustment, transfer, and return documents.

The identifier can be retrieved using the [catalog.store.list](../../store/catalog-store-list.md) method ||
|| **amount**
[`double`](../../../data-types.md) | Product quantity. The method saves a non-numeric value as `null` without an error ||
|| **purchasingPrice**
[`double`](../../../data-types.md) | Purchase price per unit in the document currency. The method saves a non-numeric value as `0` without an error ||
|#

The `elementId`, `storeFrom`, `storeTo`, `amount`, and `purchasingPrice` fields that are not passed equal `null` in the response. The method fills in the `id` field itself: it skips the passed value and unknown fields without an error.

The method does not check that the warehouses exist and match the document type. Pass warehouse identifiers as numbers: the method saves an array, for example `[43]`, as the warehouse with the identifier `1`.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"docId":371,"elementId":6595,"storeTo":43,"amount":15,"purchasingPrice":1250.5}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.element.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"docId":371,"elementId":6595,"storeTo":43,"amount":15,"purchasingPrice":1250.5},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.element.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DocumentElementResult = {
      documentElement: {
        amount: number | null
        docId: number
        elementId: number | null
        id: number
        purchasingPrice: number | null
        storeFrom: number | null
        storeTo: number | null
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<DocumentElementResult>({
        method: 'catalog.document.element.add',
        params: {
          fields: {
            docId: 371,
            elementId: 6595,
            storeTo: 43,
            amount: 15,
            purchasingPrice: 1250.5,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.documentElement)
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
      async function addDocumentElement() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.document.element.add',
            params: {
              fields: {
                docId: 371,
                elementId: 6595,
                storeTo: 43,
                amount: 15,
                purchasingPrice: 1250.5,
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
          console.info(result.documentElement)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addDocumentElement)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.element.add(
            fields={
                "docId": 371,
                "elementId": 6595,
                "storeTo": 43,
                "amount": 15,
                "purchasingPrice": 1250.5,
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

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.document.element.add',
                [
                    'fields' => [
                        'docId' => 371,
                        'elementId' => 6595,
                        'storeTo' => 43,
                        'amount' => 15,
                        'purchasingPrice' => 1250.5,
                    ],
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
        'catalog.document.element.add',
        {
            fields: {
                docId: 371,
                elementId: 6595,
                storeTo: 43,
                amount: 15,
                purchasingPrice: 1250.5
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
        'catalog.document.element.add',
        [
            'fields' => [
                'docId' => 371,
                'elementId' => 6595,
                'storeTo' => 43,
                'amount' => 15,
                'purchasingPrice' => 1250.5,
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
    res, err := client.Core().Call(ctx, "catalog.document.element.add", b24.Params{
    	"fields": b24.Params{
    		"docId":           371,
    		"elementId":       6595,
    		"storeTo":         43,
    		"amount":          15,
    		"purchasingPrice": 1250.5,
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.document.element.add: %w", err)
    }

    // The method wraps the response in an object with the "documentElement" key.
    raw, ok := b24.Unwrap(res.Result, "documentElement")
    if !ok {
    	return fmt.Errorf("no documentElement key in the response")
    }

    var item struct {
    	Amount          int     `json:"amount"`
    	DocID           b24.ID  `json:"docId"`
    	ElementID       b24.ID  `json:"elementId"`
    	ID              b24.ID  `json:"id"`
    	PurchasingPrice float64 `json:"purchasingPrice"`
    	StoreTo         int     `json:"storeTo"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.DocID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "documentElement": {
            "amount": 15,
            "docId": 371,
            "elementId": 6595,
            "id": 101,
            "purchasingPrice": 1250.5,
            "storeFrom": null,
            "storeTo": 43
        }
    },
    "time": {
        "start": 1793883862,
        "finish": 1793883862.684503,
        "duration": 0.6845030784606934,
        "processing": 0,
        "date_start": "2026-11-05T16:04:22+03:00",
        "date_finish": "2026-11-05T16:04:22+03:00",
        "operating_reset_at": 1793884440,
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
|| **result.documentElement**
[`catalog_document_element`](../../data-types.md#catalog_document_element) | Object with the fields of the added item. Fields that are not filled in are returned as `null` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_DOCUMENT_STATUS",
    "error_description": "Document not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_DOCUMENT_RIGHTS` | Access denied | The user does not have the "Create and edit" permission for documents of this type or the "View and select warehouses" permission for the item warehouse ||
|| `400` | `ERROR_DOCUMENT_STATUS` | Document not found | There is no document with the specified `docId`, or `docId` is not passed or is not a number ||
|| `400` | `ERROR_DOCUMENT_STATUS` | Conducted document | The document is processed. To change its items, cancel processing using the [catalog.document.cancel](../catalog-document-cancel.md) method ||
|| `400` | `100` | Could not find value for parameter {fields} | The `fields` parameter is not passed ||
|| `400` | `100` | Invalid value {abc} to match with parameter {fields}. Should be value of type array. | `fields` is not an object ||
|| `400` | `0` | Error of adding new document element | Failed to save the item ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-element-update.md)
- [{#T}](./catalog-document-element-list.md)
- [{#T}](./catalog-document-element-delete.md)
- [{#T}](./catalog-document-element-get-fields.md)