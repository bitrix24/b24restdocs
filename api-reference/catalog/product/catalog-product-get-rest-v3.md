# Get Product by ID v 3.0 catalog.product.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog", "View catalog products", or "Edit selling price" permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.product.get` method returns product fields by identifier.

{% note info "" %}

The method returns a product of any type: a simple product, a product with variations, a variation, or a service. The product type is returned in the `type` field.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Product identifier.

The identifier can be retrieved using the [catalog.product.list](./catalog-product-list-rest-v3.md) method ||
|| **select**
[`array`](../../data-types.md) | An array of product fields to return in the response.

Available fields:

- `id` — product identifier
- `iblockId` — information block identifier
- `name` — name
- `code` — symbolic code
- `xmlId` — external code
- `active` — activity
- `sort` — sort order
- `createdBy` — created by
- `modifiedBy` — modified by
- `dateCreate` — creation date
- `timestampX` — modification date
- `dateActiveFrom` — activity start
- `dateActiveTo` — activity end
- `previewText` — preview text
- `previewTextType` — preview text type
- `previewPicture` — preview picture
- `detailText` — detail text
- `detailTextType` — detail text type
- `detailPicture` — detail picture
- `iblockSectionId` — main section
- `iblockSection` — all sections
- `type` — product type
- `available` — availability for purchase
- `bundle` — bundle flag
- `quantity` — quantity
- `quantityReserved` — reserved quantity
- `quantityTrace` — quantity tracking
- `canBuyZero` — purchase when out of stock
- `subscribe` — subscription to the product
- `vatId` — VAT rate
- `vatIncluded` — VAT included in the price
- `purchasingPrice` — purchase price
- `purchasingCurrency` — purchase price currency
- `barcodeMulti` — barcode for each item
- `weight` — weight
- `length` — length
- `width` — width
- `height` — height
- `measure` — unit of measurement
- `recurSchemeLength` — payment period length
- `recurSchemeType` — payment period unit
- `trialPriceId` — product for trial payment
- `withoutOrder` — renewal without an order
- `quantityTraceRaw` — saved tracking mode
- `paymentType` — payment type
- `subscribeRaw` — saved subscription permission
- `canBuyZeroRaw` — saved permission to purchase when out of stock
- `properties` — values of all product properties

The `id` field is returned only if it is specified in `select`. Values of individual properties cannot be selected: `properties` returns all product properties. The `["*"]` value is not supported.

Default: all fields ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7549,"select":["id","iblockId","name","code","active","iblockSectionId","type","vatId","measure"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7549,"select":["id","iblockId","name","code","active","iblockSectionId","type","vatId","measure"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProductGetResult = {
      item: {
        id?: number
        iblockId?: number
        name?: string
        code?: string | null
        active?: 'Y' | 'N'
        iblockSectionId?: number | null
        type?: number
        vatId?: number | null
        measure?: number | null
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<ProductGetResult>({
        method: 'catalog.product.get',
        params: {
          id: 7549,
          select: ['id', 'iblockId', 'name', 'code', 'active', 'iblockSectionId', 'type', 'vatId', 'measure'],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Product:', result.item.id, result.item.name)
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
      async function productGet() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.get',
            params: {
              id: 7549,
              select: ['id', 'iblockId', 'name', 'code', 'active', 'iblockSectionId', 'type', 'vatId', 'measure'],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Product:', result.item.id, result.item.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', productGet)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.get(
            bitrix_id=7549,
            select=["id", "iblockId", "name", "code", "active", "iblockSectionId", "type", "vatId", "measure"],
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
                'catalog.product.get',
                [
                    'id' => 7549,
                    'select' => ['id', 'iblockId', 'name', 'code', 'active', 'iblockSectionId', 'type', 'vatId', 'measure'],
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
        'catalog.product.get',
        {
            id: 7549,
            select: ['id', 'iblockId', 'name', 'code', 'active', 'iblockSectionId', 'type', 'vatId', 'measure'],
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
        'catalog.product.get',
        [
            'id' => 7549,
            'select' => ['id', 'iblockId', 'name', 'code', 'active', 'iblockSectionId', 'type', 'vatId', 'measure'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.product.get", b24.Params{
    	"id":    7549,
    	"select":[]string{"id", "iblockId", "name", "code", "active", "iblockSectionId", "type", "vatId", "measure"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID         int                        `json:"id"`
    	Name       string                     `json:"name"`
    	Type       int                        `json:"type"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Name)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 7549,
            "iblockId": 25,
            "name": "Basic T-shirt, white",
            "code": "tshirt-basic",
            "active": "Y",
            "iblockSectionId": 41,
            "type": 1,
            "vatId": 1,
            "measure": 9
        }
    },
    "time": {
        "start": 1792660647,
        "finish": 1792660647.669521,
        "duration": 0.6695210933685303,
        "processing": 0,
        "date_start": "2026-10-22T12:17:27+02:00",
        "date_finish": "2026-10-22T12:17:27+02:00",
        "operating_reset_at": 1792661220,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | An object with the response data ||
|| **result.item**
[`object`](../../data-types.md) | Product fields. The set of fields depends on `select`. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Product identifier ||
|| **iblockId**
[`integer`](../../data-types.md) | Identifier of the trade catalog information block ||
|| **name**
[`string`](../../data-types.md) | Product name ||
|| **code**
[`string`](../../data-types.md) or `null` | Symbolic code ||
|| **xmlId**
[`string`](../../data-types.md) | External code. If not set on creation, it matches `id` ||
|| **active**
[`string`](../../data-types.md) | Product activity: `Y` — active, `N` — inactive ||
|| **sort**
[`integer`](../../data-types.md) | Sort order ||
|| **createdBy**
[`integer`](../../data-types.md) | Identifier of the user who created the product ||
|| **modifiedBy**
[`integer`](../../data-types.md) | Identifier of the user who last modified the product ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Creation date and time in ISO 8601 format ||
|| **timestampX**
[`datetime`](../../data-types.md) | Date and time of the last modification in ISO 8601 format ||
|| **dateActiveFrom**
[`datetime`](../../data-types.md) or `null` | Activity start date ||
|| **dateActiveTo**
[`datetime`](../../data-types.md) or `null` | Activity end date ||
|| **previewText**
[`string`](../../data-types.md) or `null` | Preview text ||
|| **previewTextType**
[`string`](../../data-types.md) | Preview text type: `text` — text, `html` — HTML ||
|| **previewPicture**
[`string`](../../data-types.md) or `null` | Identifier of the preview picture file. The file can be retrieved using the [catalog.product.download](./catalog-product-download-rest-v3.md) method ||
|| **detailText**
[`string`](../../data-types.md) or `null` | Detail text ||
|| **detailTextType**
[`string`](../../data-types.md) | Detail text type: `text` — text, `html` — HTML ||
|| **detailPicture**
[`string`](../../data-types.md) or `null` | Identifier of the detail picture file. The file can be retrieved using the [catalog.product.download](./catalog-product-download-rest-v3.md) method ||
|| **iblockSectionId**
[`integer`](../../data-types.md) or `null` | Identifier of the product's main section ||
|| **iblockSection**
[`array`](../../data-types.md) | An array of identifiers of all sections the product is linked to ||
|| **type**
[`integer`](../../data-types.md) | Product type. The list of types is returned by the [catalog.product.type.list](./catalog-product-type-list.md) method. For a product created with `catalog.product.add`, the value is `1` ||
|| **available**
[`string`](../../data-types.md) | Availability for purchase: `Y` — available, `N` — unavailable ||
|| **bundle**
[`string`](../../data-types.md) | Bundle flag: `Y` — yes, `N` — no ||
|| **quantity**
[`double`](../../data-types.md) | Available quantity ||
|| **quantityReserved**
[`double`](../../data-types.md) | Reserved quantity ||
|| **quantityTrace**
[`string`](../../data-types.md) | Effective quantity tracking mode: `Y` — enabled, `N` — disabled ||
|| **canBuyZero**
[`string`](../../data-types.md) | Effective permission to purchase an out-of-stock product: `Y` — yes, `N` — no ||
|| **subscribe**
[`string`](../../data-types.md) | Effective permission to subscribe to the product: `Y` — yes, `N` — no ||
|| **vatId**
[`integer`](../../data-types.md) or `null` | VAT rate identifier ||
|| **vatIncluded**
[`string`](../../data-types.md) | VAT is included in the price: `Y` — yes, `N` — no ||
|| **purchasingPrice**
[`double`](../../data-types.md) or `null` | Purchase price ||
|| **purchasingCurrency**
[`string`](../../data-types.md) or `null` | Purchase price currency ||
|| **barcodeMulti**
[`string`](../../data-types.md) | Unique barcode for each item: `Y` — yes, `N` — no ||
|| **weight**
[`double`](../../data-types.md) | Weight ||
|| **length**
[`double`](../../data-types.md) or `null` | Length ||
|| **width**
[`double`](../../data-types.md) or `null` | Width ||
|| **height**
[`double`](../../data-types.md) or `null` | Height ||
|| **measure**
[`integer`](../../data-types.md) or `null` | Unit of measurement identifier ||
|| **recurSchemeLength**
[`integer`](../../data-types.md) or `null` | Payment period length. Used only in the on-premise version for content sales ||
|| **recurSchemeType**
[`string`](../../data-types.md) | Time unit of the payment period. Used only in the on-premise version for content sales ||
|| **trialPriceId**
[`integer`](../../data-types.md) or `null` | Identifier of the product for trial payment. Used only in the on-premise version for content sales ||
|| **withoutOrder**
[`string`](../../data-types.md) | Renewal without placing an order: `Y` — yes, `N` — no. Used only in the on-premise version for content sales ||
|| **quantityTraceRaw**
[`string`](../../data-types.md) | Saved value of the quantity tracking mode: `Y`, `N`, or `D` — the value from the module settings ||
|| **paymentType**
[`string`](../../data-types.md) | Payment type. Used only in the on-premise version for content sales ||
|| **subscribeRaw**
[`string`](../../data-types.md) | Saved value of the subscription permission: `Y`, `N`, or `D` — the value from the module settings ||
|| **canBuyZeroRaw**
[`string`](../../data-types.md) | Saved value of the permission to purchase an out-of-stock product: `Y`, `N`, or `D` — the value from the module settings ||
|| **properties**
[`object`](../../data-types.md) | Product property values. The key is `PROPERTY_{id}`, where `id` is the property identifier. The response contains all properties of the information block.

Key value:

- `null` — the value is not filled in
- an object `{"VALUE": ..., "VALUE_ID": ...}` — the value of a single property and the identifier of this value
- an array of such objects — the values of a multiple property

For a "File" type property, `VALUE` returns the file identifier ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 99999999 was not found."
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass a positive integer. The error is also caused by `0` and a string ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | Check the identifier using the [catalog.product.list](./catalog-product-list-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `ProductDto` | Specify in `select` only the fields from the `select` parameter description. The `PROPERTY_{id}` keys and `*` are not supported ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions to view the catalog and to read the information block element ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-list-rest-v3.md)
- [{#T}](./catalog-product-update-rest-v3.md)
- [{#T}](./catalog-product-download-rest-v3.md)
- [{#T}](./catalog-product-field-list.md)
