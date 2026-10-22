# Get Parent Product Field Values v 3.0 catalog.product.sku.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog", "View catalog products", or "Edit selling price" access permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The method `catalog.product.sku.get` returns the field values of a parent product by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Parent product identifier.

The identifier can be retrieved using the [catalog.product.sku.list](./catalog-product-sku-list-rest-v3.md) method or from the [catalog.product.sku.add](./catalog-product-sku-add-rest-v3.md) response ||
|| **select**
[`array`](../../../data-types.md) | Array of parent product fields to return in the response.

Available fields:

- `id` — parent product identifier
- `iblockId` — information block identifier
- `name` — name
- `code` — symbolic code
- `xmlId` — external code
- `active` — activity
- `sort` — sort index
- `createdBy` — created by
- `modifiedBy` — modified by
- `dateCreate` — creation date
- `timestampX` — modification date
- `dateActiveFrom` — activity start
- `dateActiveTo` — activity end
- `previewText` — preview description
- `previewTextType` — preview description type
- `previewPicture` — preview picture
- `detailText` — detailed description
- `detailTextType` — detailed description type
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
- `subscribe` — subscription
- `vatId` — VAT rate
- `vatIncluded` — VAT included in the price
- `purchasingPrice` — purchase price
- `purchasingCurrency` — purchase price currency
- `barcodeMulti` — unique barcodes
- `weight` — weight
- `length` — length
- `width` — width
- `height` — height
- `measure` — unit of measurement
- `recurSchemeLength` — payment period length
- `recurSchemeType` — payment period unit
- `trialPriceId` — product for trial payment
- `withoutOrder` — renewal without an order
- `quantityTraceRaw` — saved quantity tracking value
- `paymentType` — payment type
- `subscribeRaw` — saved subscription value
- `canBuyZeroRaw` — saved value of purchase when out of stock
- `properties` — property values

The `id` field is returned only if it is specified in `select`. Values of individual properties cannot be selected: `properties` returns all properties. The `["*"]` value is not supported.

Default: all fields, including `properties` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.sku.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7553,"select":["id","iblockId","name","code","active","type","iblockSectionId","previewText","detailText","detailPicture","dateCreate"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.sku.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7553,"select":["id","iblockId","name","code","active","type","iblockSectionId","previewText","detailText","detailPicture","dateCreate"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.sku.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type SkuGetResult = {
      item: {
        id: number
        name: string
        type: number
        detailPicture: string | null
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<SkuGetResult>({
        method: 'catalog.product.sku.get',
        params: {
          id: 7553,
          select: ['id', 'iblockId', 'name', 'code', 'active', 'type', 'iblockSectionId', 'previewText', 'detailText', 'detailPicture', 'dateCreate'],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Item:', result.item)
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
      async function getSku() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.sku.get',
            params: {
              id: 7553,
              select: ['id', 'iblockId', 'name', 'code', 'active', 'type', 'iblockSectionId', 'previewText', 'detailText', 'detailPicture', 'dateCreate'],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Item:', result.item)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getSku)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.sku.get(
            id=7553,
            select=["id", "iblockId", "name", "code", "active", "type", "iblockSectionId", "previewText", "detailText", "detailPicture", "dateCreate"],
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
                'catalog.product.sku.get',
                [
                    'id' => 7553,
                    'select' => ['id', 'iblockId', 'name', 'code', 'active', 'type', 'iblockSectionId', 'previewText', 'detailText', 'detailPicture', 'dateCreate'],
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
        'catalog.product.sku.get',
        {
            id: 7553,
            select: ['id', 'iblockId', 'name', 'code', 'active', 'type', 'iblockSectionId', 'previewText', 'detailText', 'detailPicture', 'dateCreate'],
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
        'catalog.product.sku.get',
        [
            'id' => 7553,
            'select' => ['id', 'iblockId', 'name', 'code', 'active', 'type', 'iblockSectionId', 'previewText', 'detailText', 'detailPicture', 'dateCreate'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.product.sku.get", b24.Params{
    	"id": 7553,
    	"select": []string{"id", "iblockId", "name", "code", "active", "type", "iblockSectionId", "previewText", "detailText", "detailPicture", "dateCreate"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.sku.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var item struct {
    	ID   int    `json:"id"`
    	Name string `json:"name"`
    	Type int    `json:"type"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
    }
    fmt.Println(item.ID, item.Name, item.Type)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 7553,
            "iblockId": 25,
            "name": "DOCS-V3-SKU Cotton T-shirt",
            "code": "docs-v3-sku-t-shirt",
            "active": "Y",
            "dateCreate": "2026-10-22T12:18:48+02:00",
            "previewText": "Cotton T-shirt",
            "detailText": "Composition: 100% cotton",
            "detailPicture": "39455",
            "iblockSectionId": 31,
            "type": 3
        }
    },
    "time": {
        "start": 1792660764,
        "finish": 1792660764.959464,
        "duration": 0.9594640731811523,
        "processing": 0,
        "date_start": "2026-10-22T12:19:24+02:00",
        "date_finish": "2026-10-22T12:19:24+02:00",
        "operating_reset_at": 1792661340,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with the response data ||
|| **result.item**
[`object`](../../../data-types.md) | Parent product fields. The set depends on `select`. [Field description](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Parent product identifier ||
|| **iblockId**
[`integer`](../../../data-types.md) | Identifier of the product catalog information block ||
|| **name**
[`string`](../../../data-types.md) | Parent product name ||
|| **code**
[`string`](../../../data-types.md) or `null` | Symbolic code ||
|| **xmlId**
[`string`](../../../data-types.md) | External code. If not set, it matches `id` ||
|| **active**
[`string`](../../../data-types.md) | Activity: `Y` — active, `N` — inactive ||
|| **sort**
[`integer`](../../../data-types.md) | Sort index ||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the user who created the record ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | Identifier of the user who last modified the record ||
|| **dateCreate**
[`datetime`](../../../data-types.md) | Creation date and time ||
|| **timestampX**
[`datetime`](../../../data-types.md) | Date and time of the last modification ||
|| **dateActiveFrom**
[`datetime`](../../../data-types.md) or `null` | Activity start date ||
|| **dateActiveTo**
[`datetime`](../../../data-types.md) or `null` | Activity end date ||
|| **previewText**
[`string`](../../../data-types.md) or `null` | Preview description ||
|| **previewTextType**
[`string`](../../../data-types.md) | Preview description type: `text` or `html` ||
|| **previewPicture**
[`string`](../../../data-types.md) or `null` | Preview picture file identifier ||
|| **detailText**
[`string`](../../../data-types.md) or `null` | Detailed description ||
|| **detailTextType**
[`string`](../../../data-types.md) | Detailed description type: `text` or `html` ||
|| **detailPicture**
[`string`](../../../data-types.md) or `null` | Detail picture file identifier ||
|| **iblockSectionId**
[`integer`](../../../data-types.md) or `null` | Main section identifier ||
|| **iblockSection**
[`array`](../../../data-types.md) | Array of identifiers of all sections the product is linked to ||
|| **type**
[`integer`](../../../data-types.md) | Product type: `3` — parent product with variations, `6` — without variations ||
|| **available**
[`string`](../../../data-types.md) | Availability for purchase: `Y` — available, `N` — unavailable ||
|| **bundle**
[`string`](../../../data-types.md) | Whether the product has a bundle: `Y` — yes, `N` — no ||
|| **quantity**
[`double`](../../../data-types.md) | Available quantity ||
|| **quantityReserved**
[`double`](../../../data-types.md) | Reserved quantity ||
|| **quantityTrace**
[`string`](../../../data-types.md) | Whether quantity tracking is enabled: `Y` — yes, `N` — no ||
|| **canBuyZero**
[`string`](../../../data-types.md) | Whether purchase is allowed when the product is out of stock: `Y` — yes, `N` — no ||
|| **subscribe**
[`string`](../../../data-types.md) | Whether subscription to the product is allowed: `Y` — yes, `N` — no ||
|| **vatId**
[`integer`](../../../data-types.md) or `null` | VAT rate identifier ||
|| **vatIncluded**
[`string`](../../../data-types.md) | Whether VAT is included in the price: `Y` — yes, `N` — no ||
|| **purchasingPrice**
[`double`](../../../data-types.md) or `null` | Purchase price. Returned if the user has the "View purchase price" access permission ||
|| **purchasingCurrency**
[`string`](../../../data-types.md) or `null` | Purchase price currency. Returned if the user has the "View purchase price" access permission ||
|| **barcodeMulti**
[`string`](../../../data-types.md) | Unique barcodes for each item: `Y` — yes, `N` — no ||
|| **weight**
[`double`](../../../data-types.md) or `null` | Weight ||
|| **length**
[`double`](../../../data-types.md) or `null` | Length ||
|| **width**
[`double`](../../../data-types.md) or `null` | Width ||
|| **height**
[`double`](../../../data-types.md) or `null` | Height ||
|| **measure**
[`integer`](../../../data-types.md) or `null` | Unit of measurement identifier ||
|| **recurSchemeLength**
[`integer`](../../../data-types.md) or `null` | Payment period length. Used only in the on-premise version for content sales ||
|| **recurSchemeType**
[`string`](../../../data-types.md) | Time unit of the payment period. Used only in the on-premise version for content sales ||
|| **trialPriceId**
[`integer`](../../../data-types.md) or `null` | Product for trial payment. Used only in the on-premise version for content sales ||
|| **withoutOrder**
[`string`](../../../data-types.md) | Renewal without placing an order: `Y` — yes, `N` — no. Used only in the on-premise version for content sales ||
|| **quantityTraceRaw**
[`string`](../../../data-types.md) | Saved quantity tracking value: `Y` — yes, `N` — no, `D` — according to the module settings ||
|| **paymentType**
[`string`](../../../data-types.md) | Payment type: `S` — one-time, `R` — recurring, `T` — trial. Used only in the on-premise version for content sales ||
|| **subscribeRaw**
[`string`](../../../data-types.md) | Saved subscription value: `Y` — yes, `N` — no, `D` — according to the module settings ||
|| **canBuyZeroRaw**
[`string`](../../../data-types.md) | Saved value of purchase when out of stock: `Y` — yes, `N` — no, `D` — according to the module settings ||
|| **properties**
[`object`](../../../data-types.md) | Product property values. The keys are `PROPERTY_{id}`, where `{id}` is the property identifier. The value is `null`, a `{"VALUE": ..., "VALUE_ID": ...}` object, or an array of such objects for a multiple property ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 7533 was not found."
    }
}
```

{% include notitle [error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass a positive number in `id` ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | The record does not exist or is not a parent product. Retrieve variations using the [catalog.product.offer.get](../offer/catalog-product-offer-get-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `ProductDto` | Specify in `select` only the fields from the `select` parameter description. For property values, use `properties` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions for the catalog and the information block ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-sku-add-rest-v3.md)
- [{#T}](./catalog-product-sku-update-rest-v3.md)
- [{#T}](./catalog-product-sku-list-rest-v3.md)
- [{#T}](./catalog-product-sku-delete-rest-v3.md)
- [{#T}](./catalog-product-sku-download-rest-v3.md)
- [{#T}](./catalog-product-sku-field-list.md)
- [{#T}](./catalog-product-sku-field-get.md)
