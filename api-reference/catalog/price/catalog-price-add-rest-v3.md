# Add Product Price v 3.0 catalog.price.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Edit selling price" access permission and permission to modify product prices in the catalog information block

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The method `catalog.price.add` adds a product price and returns its fields.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Object with the fields of the new price. [Field description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **productId***
[`integer`](../../data-types.md) | Identifier of the product or variation.

The identifier can be retrieved using the [catalog.product.list](../product/catalog-product-list.md) method.

After the price is created, `productId` cannot be changed ||
|| **catalogGroupId***
[`integer`](../../data-types.md) | Price type identifier.

The identifier can be retrieved using the [catalog.pricetype.list](../price-type/catalog-price-type-list-rest-v3.md) method.

After the price is created, `catalogGroupId` cannot be changed ||
|| **price***
[`double`](../../data-types.md) | Price. A negative value causes an error.

If `extraId` is passed and the price type is not the base one, the price is recalculated from the product's base price taking the markup into account ||
|| **currency***
[`string`](../../data-types.md) | Currency code, for example `EUR`. No longer than three characters.

Currency codes can be retrieved using the [crm.currency.list](../../crm/currency/crm-currency-list.md) method ||
|| **extraId**
[`integer`](../../data-types.md) | Markup identifier.

The identifier can be retrieved using the [catalog.extra.list](../extra/catalog-extra-list-rest-v3.md) method.

The markup is not applied to the base price type, and the value is reset to `0`. A non-existent identifier is also reset to `0` without an error ||
|| **quantityFrom**
[`integer`](../../data-types.md) | Lower bound of the product quantity range in which the price applies. `0` — no lower bound.

The ranges of all price types of a product must match the ranges of the base price ||
|| **quantityTo**
[`integer`](../../data-types.md) | Upper bound of the product quantity range in which the price applies. `0` — no upper bound ||
|#

Without quantity ranges, a product can have only one price of each type.

The `id`, `priceScale`, and `timestampX` fields are filled in automatically. If you pass them in `fields`, the method returns an error.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.price.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"productId":7525,"catalogGroupId":1,"price":1500,"currency":"EUR"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.price.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"productId":7525,"catalogGroupId":1,"price":1500,"currency":"EUR"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.price.add
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type PriceAddResult = {
      item: {
        id: number
        productId: number
        catalogGroupId: number
        price: number
        currency: string
        extraId: number
        quantityFrom: number
        quantityTo: number
        priceScale: number
        timestampX: ISODate
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<PriceAddResult>({
        method: 'catalog.price.add',
        params: {
          fields: {
            productId: 7525,
            catalogGroupId: 1,
            price: 1500,
            currency: 'EUR',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Price created:', result.item.id)
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
      async function addPrice() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.price.add',
            params: {
              fields: {
                productId: 7525,
                catalogGroupId: 1,
                price: 1500,
                currency: 'EUR',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Price created:', result.item.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addPrice)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.price.add(
            fields={
                "productId": 7525,
                "catalogGroupId": 1,
                "price": 1500,
                "currency": "EUR",
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
                'catalog.price.add',
                [
                    'fields' => [
                        'productId' => 7525,
                        'catalogGroupId' => 1,
                        'price' => 1500,
                        'currency' => 'EUR',
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
        'catalog.price.add',
        {
            fields: {
                productId: 7525,
                catalogGroupId: 1,
                price: 1500,
                currency: 'EUR',
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
        'catalog.price.add',
        [
            'fields' => [
                'productId' => 7525,
                'catalogGroupId' => 1,
                'price' => 1500,
                'currency' => 'EUR',
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
    res, err := client.Core().Call(ctx, "catalog.price.add", b24.Params{
    	"fields": b24.Params{
    		"productId":      7525,
    		"catalogGroupId": 1,
    		"price":          1500,
    		"currency":       "EUR",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.price.add: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var item struct {
    	ID             int     `json:"id"`
    	ProductID      int     `json:"productId"`
    	CatalogGroupID int     `json:"catalogGroupId"`
    	Price          float64 `json:"price"`
    	Currency       string  `json:"currency"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Price, item.Currency)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 891,
            "productId": 7525,
            "catalogGroupId": 1,
            "price": 1500,
            "currency": "EUR",
            "extraId": 0,
            "quantityFrom": 0,
            "quantityTo": 0,
            "priceScale": 1500,
            "timestampX": "2026-10-22T14:19:56+02:00"
        }
    },
    "time": {
        "start": 1792667996,
        "finish": 1792667996.347515,
        "duration": 0.3475151062011719,
        "processing": 0,
        "date_start": "2026-10-22T14:19:56+02:00",
        "date_finish": "2026-10-22T14:19:56+02:00",
        "operating_reset_at": 1792668540,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the response data ||
|| **result.item**
[`object`](../../data-types.md) | Fields of the created price. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Price identifier ||
|| **productId**
[`integer`](../../data-types.md) | Identifier of the product or variation ||
|| **catalogGroupId**
[`integer`](../../data-types.md) | Price type identifier ||
|| **price**
[`double`](../../data-types.md) | Price ||
|| **currency**
[`string`](../../data-types.md) | Currency code ||
|| **extraId**
[`integer`](../../data-types.md) | Markup identifier. `0` if no markup is set ||
|| **quantityFrom**
[`integer`](../../data-types.md) | Lower bound of the quantity range. `0` if the bound is not set ||
|| **quantityTo**
[`integer`](../../data-types.md) | Upper bound of the quantity range. `0` if the bound is not set ||
|| **priceScale**
[`double`](../../data-types.md) | Price in the base currency. Calculated automatically ||
|| **timestampX**
[`datetime`](../../data-types.md) | Date and time of the last price change ||
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
                "message": "Validate price error. Catalog product is allowed has only single price without ranges in price group.",
                "field": "catalogGroupId"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `price` | This request requires data type `float` in field `price` | Pass a number ||
|| `catalogGroupId` | Validate price error. Catalog price group is wrong | There is no price type with this identifier. Check the identifier using the [catalog.pricetype.list](../price-type/catalog-price-type-list-rest-v3.md) method ||
|| `catalogGroupId` | Validate price error. Catalog product is allowed has only single price without ranges in price group. | The product already has a price of this type. Change it using the [catalog.price.update](./catalog-price-update-rest-v3.md) method ||
|| `quantityFrom` | Validate price error. Ranges of base price are not equal to another price group range | Set the quantity ranges the same as for the product's base price ||
|| `quantityTo` | Invalid quantity range entry: no price is specified for quantity over `#QUANTITY#` | Add a price for the quantity above the upper bound or pass `quantityTo` equal to `0` ||
|| `BX_ERROR` | Invalid price | Pass a number of `0` or greater in `price` ||
|| `BX_ERROR` | Invalid currency | Pass the code of an existing currency ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `productId` | Required field "productId" is empty | Pass `productId` in `fields` ||
|| `catalogGroupId` | Required field "catalogGroupId" is empty | Pass `catalogGroupId` in `fields` ||
|| `price` | Required field "price" is empty | Pass `price` in `fields` ||
|| `currency` | Required field "currency" is empty | Pass `currency` in `fields` ||
|| `currency` | String must not be longer than 3 characters | Pass a three-letter currency code ||
|| `#FIELD#` | Field "#FIELD#" is read-only | Remove the `id`, `priceScale`, and `timestampX` fields from `fields` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `PriceDto` | Pass in `fields` only the fields from the [Parameter fields](#fields) table ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions to modify prices. The error is also returned if there is no product with the `productId` identifier ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-update-rest-v3.md)
- [{#T}](./catalog-price-get-rest-v3.md)
- [{#T}](./catalog-price-list-rest-v3.md)
- [{#T}](./catalog-price-delete-rest-v3.md)
- [{#T}](./catalog-price-field-list.md)
