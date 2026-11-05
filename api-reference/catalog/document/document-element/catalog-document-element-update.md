# Update Product in Inventory Document catalog.document.element.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Create and edit" permission for documents of this type and the "View and select warehouses" permission for the item warehouses

The method `catalog.document.element.update` updates a product item in an unprocessed inventory document and returns its fields after the update.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_document_element.id`](../../data-types.md#catalog_document_element) | Identifier of the product item.

The identifier can be retrieved using the [catalog.document.element.list](./catalog-document-element-list.md) method ||
|| **fields***
[`object`](../../../data-types.md) | Item fields to update ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **storeFrom**
[`catalog_store.id`](../../data-types.md#catalog_store) | Identifier of the write-off warehouse. Filled in for transfer and write-off documents.

The identifier can be retrieved using the [catalog.store.list](../../store/catalog-store-list.md) method ||
|| **storeTo**
[`catalog_store.id`](../../data-types.md#catalog_store) | Identifier of the receipt warehouse. Filled in for receipt, stock adjustment, transfer, and return documents.

The identifier can be retrieved using the [catalog.store.list](../../store/catalog-store-list.md) method ||
|| **amount**
[`double`](../../../data-types.md) | Product quantity. The method saves a non-numeric value as `null` without an error, and the previous quantity is erased ||
|| **purchasingPrice**
[`double`](../../../data-types.md) | Purchase price per unit in the document currency. The method saves a non-numeric value as `0` without an error ||
|#

The method changes only the passed fields; the other fields retain their values.

The `docId` and `elementId` fields cannot be changed: the method skips them without an error. To replace the product or move the item to another document, delete the item using the [catalog.document.element.delete](./catalog-document-element-delete.md) method and add a new one using the [catalog.document.element.add](./catalog-document-element-add.md) method.

If `fields` contains no updatable field — it is an empty object or contains only `docId`, `elementId`, or unknown fields — the method returns the `Error of modifying new document` error.

The method does not check that the warehouses exist and match the document type.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":101,"fields":{"amount":12,"purchasingPrice":1180,"storeTo":43}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.element.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":101,"fields":{"amount":12,"purchasingPrice":1180,"storeTo":43},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.element.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DocumentElementUpdateResult = {
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
      const response = await $b24.actions.v2.call.make<DocumentElementUpdateResult>({
        method: 'catalog.document.element.update',
        params: {
          id: 101,
          fields: {
            amount: 12,
            purchasingPrice: 1180,
            storeTo: 43,
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
      async function updateDocumentElement() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.document.element.update',
            params: {
              id: 101,
              fields: {
                amount: 12,
                purchasingPrice: 1180,
                storeTo: 43,
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

      document.addEventListener('DOMContentLoaded', updateDocumentElement)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.element.update(
            bitrix_id=101,
            fields={
                "amount": 12,
                "purchasingPrice": 1180,
                "storeTo": 43,
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
                'catalog.document.element.update',
                [
                    'id' => 101,
                    'fields' => [
                        'amount' => 12,
                        'purchasingPrice' => 1180,
                        'storeTo' => 43,
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
        'catalog.document.element.update',
        {
            id: 101,
            fields: {
                amount: 12,
                purchasingPrice: 1180,
                storeTo: 43
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
        'catalog.document.element.update',
        [
            'id' => 101,
            'fields' => [
                'amount' => 12,
                'purchasingPrice' => 1180,
                'storeTo' => 43,
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
    res, err := client.Core().Call(ctx, "catalog.document.element.update", b24.Params{
    	"id": 101,
    	"fields": b24.Params{
    		"amount":          12,
    		"purchasingPrice": 1180,
    		"storeTo":         43,
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.document.element.update: %w", err)
    }

    // The method wraps the response in an object with the "documentElement" key.
    raw, ok := b24.Unwrap(res.Result, "documentElement")
    if !ok {
    	return fmt.Errorf("no documentElement key in the response")
    }

    // Fields that are not filled in are returned as null, so they use pointers.
    var item struct {
    	Amount          *float64 `json:"amount"`
    	DocID           b24.ID   `json:"docId"`
    	ElementID       *int     `json:"elementId"`
    	ID              b24.ID   `json:"id"`
    	PurchasingPrice *float64 `json:"purchasingPrice"`
    	StoreFrom       *int     `json:"storeFrom"`
    	StoreTo         *int     `json:"storeTo"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Amount)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "documentElement": {
            "amount": 12,
            "docId": 371,
            "elementId": 6595,
            "id": 101,
            "purchasingPrice": 1180,
            "storeFrom": null,
            "storeTo": 43
        }
    },
    "time": {
        "start": 1793883948,
        "finish": 1793883948.402954,
        "duration": 0.4029541015625,
        "processing": 0,
        "date_start": "2026-11-05T16:05:48+03:00",
        "date_finish": "2026-11-05T16:05:48+03:00",
        "operating_reset_at": 1793884500,
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
[`catalog_document_element`](../../data-types.md#catalog_document_element) | Object with all item fields after the update. Fields that are not filled in are returned as `null` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Error of modifying new document"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_DOCUMENT_RIGHTS` | Access denied | The user does not have the "Create and edit" permission for documents of this type or the "View and select warehouses" permission for the item warehouse ||
|| `400` | `ERROR_DOCUMENT_STATUS` | Document not found | There is no item with the specified `id` ||
|| `400` | `ERROR_DOCUMENT_STATUS` | Conducted document | The document is processed. To change its items, cancel processing using the [catalog.document.cancel](../catalog-document-cancel.md) method ||
|| `400` | `100` | Could not find value for parameter {id} | The `id` parameter is not passed ||
|| `400` | `100` | Invalid value {abc} to match with parameter {id}. Should be value of type int. | `id` is not a number ||
|| `400` | `100` | Could not find value for parameter {fields} | The `fields` parameter is not passed ||
|| `400` | `0` | Error of modifying new document | `fields` contains no updatable field, or the item could not be saved ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-element-add.md)
- [{#T}](./catalog-document-element-list.md)
- [{#T}](./catalog-document-element-delete.md)
- [{#T}](./catalog-document-element-get-fields.md)