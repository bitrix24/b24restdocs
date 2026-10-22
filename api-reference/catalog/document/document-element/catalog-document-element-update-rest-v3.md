# Update Product in Inventory Document v 3.0 catalog.document.element.update

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

The `catalog.document.element.update` method updates a product item in an unprocessed inventory document and returns its fields.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the product item.

The identifier can be retrieved using the [catalog.document.element.list](./catalog-document-element-list-rest-v3.md) method or from the [catalog.document.element.add](./catalog-document-element-add-rest-v3.md) response ||
|| **fields***
[`object`](../../../data-types.md) | Object with the product item fields to update. Pass only the fields being changed. [Field description](#fields)

An empty object `{}` causes an error ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
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

{% note warning "" %}

According to the field description, `docId` and `elementId` are set only when adding. Currently the method also accepts them in `fields` and moves the item to another document or changes the product. To change the document or the product, remove the item and add a new one.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.document.element.update`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":67,"fields":{"amount":4,"purchasingPrice":110}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.element.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":67,"fields":{"amount":4,"purchasingPrice":110},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.document.element.update
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentElementUpdateResult = {
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
      const response = await $b24.actions.v3.call.make<DocumentElementUpdateResult>({
        method: 'catalog.document.element.update',
        params: {
          id: 67,
          fields: {
            amount: 4,
            purchasingPrice: 110,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Element updated:', result.item.amount)
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
      async function documentElementUpdate() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.document.element.update',
            params: {
              id: 67,
              fields: {
                amount: 4,
                purchasingPrice: 110,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Element updated:', result.item.amount)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', documentElementUpdate)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.element.update(
            bitrix_id=67,
            fields={
                "amount": 4,
                "purchasingPrice": 110,
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
                'catalog.document.element.update',
                [
                    'id' => 67,
                    'fields' => [
                        'amount' => 4,
                        'purchasingPrice' => 110,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'catalog.document.element.update',
        {
            id: 67,
            fields: {
                amount: 4,
                purchasingPrice: 110,
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
        'catalog.document.element.update',
        [
            'id' => 67,
            'fields' => [
                'amount' => 4,
                'purchasingPrice' => 110,
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
    	"id":     67,
    	"fields": b24.Params{
    		"amount":          4,
    		"purchasingPrice": 110,
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.document.element.update: %w", err)
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
            "amount": 4,
            "purchasingPrice": 110
        }
    },
    "time": {
        "start": 1792665370,
        "finish": 1792665370.918465,
        "duration": 0.9184648990631104,
        "processing": 0,
        "date_start": "2026-10-22T13:36:10+02:00",
        "date_finish": "2026-10-22T13:36:10+02:00",
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
[`object`](../../../data-types.md) | Fields of the product item after the update. [Field description](#item) ||
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
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 999999 was not found."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | At least one of the required fields must be not empty: id, filter | Pass the `id` of the product item. This message comes together with a second one: "Only one of the following values must be provided: id, filter" ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `amount` | This request requires data type `float` in field `amount` | Pass a number ||
|| `ERROR_DOCUMENT_STATUS` | Conducted document | The document is processed. Cancel processing using the [catalog.document.cancel](../catalog-document-cancel-rest-v3.md) method and repeat the request ||
|| `-` | Error of modifying new document | Pass at least one field in `fields` ||
|#

#### Filter Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `filter` | The method does not support `filter`. Pass the `id` of the product item ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | There is no product item with this identifier. Check the identifier using the [catalog.document.element.list](./catalog-document-element-list-rest-v3.md) method ||
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

- [{#T}](./catalog-document-element-add-rest-v3.md)
- [{#T}](./catalog-document-element-list-rest-v3.md)
- [{#T}](./catalog-document-element-delete-rest-v3.md)
- [{#T}](./catalog-document-element-field-list.md)
- [{#T}](./catalog-document-element-field-get.md)
- [{#T}](../catalog-document-list-rest-v3.md)
