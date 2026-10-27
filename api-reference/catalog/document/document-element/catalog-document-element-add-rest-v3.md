# Add Product to Inventory Document v 3.0 catalog.document.element.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Create and edit" permission for documents of this type and the "View and select warehouses" permission for the warehouses of the product item

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `catalog.document.element.add` method adds a product item to an inventory document and returns its fields.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object with the fields of the new product item. [Field description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **docId***
[`integer`](../../../data-types.md) | Identifier of the inventory document. The document must not be processed.

The identifier can be retrieved using the [catalog.document.list](../catalog-document-list-rest-v3.md) method or from the [catalog.document.add](../catalog-document-add-rest-v3.md) response ||
|| **elementId***
[`integer`](../../../data-types.md) | Product identifier.

The identifier can be retrieved using the [catalog.product.list](../../product/catalog-product-list-rest-v3.md) method ||
|| **storeFrom**
[`integer`](../../../data-types.md) | Identifier of the source warehouse the product is written off from. Used in write-off, sale, and transfer documents.

The identifier can be retrieved using the [catalog.store.list](../../store/catalog-store-list.md) method ||
|| **storeTo**
[`integer`](../../../data-types.md) | Identifier of the receiving warehouse. Used in receipt, stock adjustment, and transfer documents.

The identifier can be retrieved using the [catalog.store.list](../../store/catalog-store-list.md) method ||
|| **amount**
[`double`](../../../data-types.md) | Product quantity ||
|| **purchasingPrice**
[`double`](../../../data-types.md) | Purchase price in the document currency ||
|#

The `id` field is filled in automatically. If you pass it in `fields`, the method returns an error.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.document.element.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"docId":325,"storeTo":17,"elementId":7573,"amount":3,"purchasingPrice":115}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.element.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"docId":325,"storeTo":17,"elementId":7573,"amount":3,"purchasingPrice":115},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.document.element.add
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentElementAddResult = {
      item: {
        id: number
        docId: number
        storeFrom: number
        storeTo: number
        elementId: number
        amount: number
        purchasingPrice: number
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<DocumentElementAddResult>({
        method: 'catalog.document.element.add',
        params: {
          fields: {
            docId: 325,
            storeTo: 17,
            elementId: 7573,
            amount: 3,
            purchasingPrice: 115,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Element added:', result.item.id)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function documentElementAdd() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.document.element.add',
            params: {
              fields: {
                docId: 325,
                storeTo: 17,
                elementId: 7573,
                amount: 3,
                purchasingPrice: 115,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Element added:', result.item.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', documentElementAdd)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.element.add(
            fields={
                "docId": 325,
                "storeTo": 17,
                "elementId": 7573,
                "amount": 3,
                "purchasingPrice": 115,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.document.element.add',
                [
                    'fields' => [
                        'docId' => 325,
                        'storeTo' => 17,
                        'elementId' => 7573,
                        'amount' => 3,
                        'purchasingPrice' => 115,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'catalog.document.element.add',
        {
            fields: {
                docId: 325,
                storeTo: 17,
                elementId: 7573,
                amount: 3,
                purchasingPrice: 115,
            },
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.document.element.add',
        [
            'fields' => [
                'docId' => 325,
                'storeTo' => 17,
                'elementId' => 7573,
                'amount' => 3,
                'purchasingPrice' => 115,
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
    		"docId":           325,
    		"storeTo":         17,
    		"elementId":       7573,
    		"amount":          3,
    		"purchasingPrice": 115,
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.document.element.add: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID        int     `json:"id"`
    	DocID     int     `json:"docId"`
    	ElementID int     `json:"elementId"`
    	Amount    float64 `json:"amount"`
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
        "item": {
            "id": 67,
            "docId": 325,
            "storeFrom": 0,
            "storeTo": 17,
            "elementId": 7573,
            "amount": 3,
            "purchasingPrice": 115
        }
    },
    "time": {
        "start": 1792665369,
        "finish": 1792665369.641646,
        "duration": 0.6416459083557129,
        "processing": 0,
        "date_start": "2026-10-22T13:36:09+02:00",
        "date_finish": "2026-10-22T13:36:09+02:00",
        "operating_reset_at": 1792665960,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Response data object ||
|| **result.item**
[`object`](../../../data-types.md) | Fields of the added product item. [Field description](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the document product item ||
|| **docId**
[`integer`](../../../data-types.md) | Identifier of the inventory document ||
|| **storeFrom**
[`integer`](../../../data-types.md) | Identifier of the source warehouse. `0` — no warehouse specified ||
|| **storeTo**
[`integer`](../../../data-types.md) | Identifier of the receiving warehouse. `0` — no warehouse specified ||
|| **elementId**
[`integer`](../../../data-types.md) | Product identifier ||
|| **amount**
[`double`](../../../data-types.md) | Product quantity ||
|| **purchasingPrice**
[`double`](../../../data-types.md) | Purchase price in the document currency ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "Conducted document",
                "field": "ERROR_DOCUMENT_STATUS"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `docId` | Required field `docId` is empty | Pass `docId` in `fields` ||
|| `elementId` | Required field `elementId` is empty | Pass `elementId` in `fields` ||
|| `id` | Field `id` is read-only | Remove `id` from `fields` ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `docId` | This request requires data type `int` in field `docId` | Pass an integer ||
|| `amount` | This request requires data type `float` in field `amount` | Pass a number ||
|| `ERROR_DOCUMENT_STATUS` | Conducted document | The document is processed. Cancel processing using the [catalog.document.cancel](../catalog-document-cancel-rest-v3.md) method and repeat the request ||
|| `-` | Error of adding new document element | The product item was not saved. Check the field values and repeat the request ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | There is no document with the `docId` identifier. Check the identifier using the [catalog.document.list](../catalog-document-list-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `DocumentElementDto` | Pass in `fields` only the fields from the [Parameter fields](#fields) table ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the permissions for documents of this type and for the warehouses of the product item ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-element-update-rest-v3.md)
- [{#T}](./catalog-document-element-list-rest-v3.md)
- [{#T}](./catalog-document-element-delete-rest-v3.md)
- [{#T}](./catalog-document-element-field-list.md)
- [{#T}](./catalog-document-element-field-get.md)
- [{#T}](../catalog-document-list-rest-v3.md)
