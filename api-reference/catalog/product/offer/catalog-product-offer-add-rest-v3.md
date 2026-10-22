# Add Product Variation v 3.0 catalog.product.offer.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Create product" permission and one of the permissions: "View product catalog", "View catalog products", or "Edit selling price"

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The method `catalog.product.offer.add` creates a variation in the offers catalog and returns its fields.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object with the fields of the new variation. [Field description](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **iblockId***
[`integer`](../../../data-types.md) | Identifier of the offers catalog information block.

The identifier can be retrieved using the [catalog.catalog.list](../../catalog/catalog-catalog-list-rest-v3.md) method: it is the catalog whose `productIblockId` is greater than `0` ||
|| **name***
[`string`](../../../data-types.md) | Variation name ||
|| **code**
[`string`](../../../data-types.md) | Symbolic code ||
|| **xmlId**
[`string`](../../../data-types.md) | External code. If not passed on creation, it matches `id` ||
|| **active**
[`string`](../../../data-types.md) | Activity flag.

Possible values:

- `Y` — active
- `N` — inactive

Any value other than `Y` is saved as `N`.

Default: `Y` ||
|| **sort**
[`integer`](../../../data-types.md) | Sort order.

Default: `500` ||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the user who created the record. The identifier can be retrieved using the [user.get](../../../user/user-get.md) method ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | Identifier of the user who modified the record. Defaults to the current user ||
|| **dateActiveFrom**
[`datetime`](../../../data-types.md) | Activity start date in ISO 8601 format ||
|| **dateActiveTo**
[`datetime`](../../../data-types.md) | Activity end date in ISO 8601 format ||
|| **previewText**
[`string`](../../../data-types.md) | Preview description ||
|| **previewTextType**
[`string`](../../../data-types.md) | Preview description type.

Possible values:

- `text` — text
- `html` — HTML

Default: `text` ||
|| **previewPicture**
[`object`](../../../data-types.md) | Preview picture. The method currently does not save the field value, see the warning below for details ||
|| **detailText**
[`string`](../../../data-types.md) | Detailed description ||
|| **detailTextType**
[`string`](../../../data-types.md) | Detailed description type.

Possible values:

- `text` — text
- `html` — HTML

Default: `text` ||
|| **detailPicture**
[`object`](../../../data-types.md) | Detail picture. The method currently does not save the field value, see the warning below for details ||
|| **iblockSectionId**
[`integer`](../../../data-types.md) | Identifier of the main section.

The identifier can be retrieved using the [catalog.section.list](../../section/catalog-section-list-rest-v3.md) method. For a non-existent section, the method returns an access error ||
|| **iblockSection**
[`array`](../../../data-types.md) | Array of identifiers of all sections to link the product to ||
|| **quantity**
[`double`](../../../data-types.md) | Available quantity. The value is not saved when inventory management is enabled ||
|| **quantityReserved**
[`double`](../../../data-types.md) | Reserved quantity ||
|| **quantityTrace**
[`string`](../../../data-types.md) | Quantity tracking.

Possible values:

- `Y` — enabled
- `N` — disabled

Always `Y` when inventory management is enabled ||
|| **canBuyZero**
[`string`](../../../data-types.md) | Allow purchase when the product is out of stock.

Possible values:

- `Y` — yes
- `N` — no ||
|| **subscribe**
[`string`](../../../data-types.md) | Allow subscription to the product.

Possible values:

- `Y` — yes
- `N` — no ||
|| **vatId**
[`integer`](../../../data-types.md) | VAT rate identifier. The identifier can be retrieved using the [catalog.vat.list](../../vat/catalog-vat-list-rest-v3.md) method ||
|| **vatIncluded**
[`string`](../../../data-types.md) | Whether VAT is included in the price.

Possible values:

- `Y` — yes
- `N` — no ||
|| **purchasingPrice**
[`double`](../../../data-types.md) | Purchase price. The value is not saved when inventory management is enabled ||
|| **purchasingCurrency**
[`string`](../../../data-types.md) | Purchase price currency, for example `EUR`. The list of currencies can be retrieved using the [crm.currency.list](../../../crm/currency/crm-currency-list.md) method. The value is not saved when inventory management is enabled ||
|| **barcodeMulti**
[`string`](../../../data-types.md) | Unique barcodes for each item.

Possible values:

- `Y` — yes
- `N` — no ||
|| **weight**
[`double`](../../../data-types.md) | Weight ||
|| **length**
[`double`](../../../data-types.md) | Length ||
|| **width**
[`double`](../../../data-types.md) | Width ||
|| **height**
[`double`](../../../data-types.md) | Height ||
|| **measure**
[`integer`](../../../data-types.md) | Unit of measurement identifier. The identifier can be retrieved using the [catalog.measure.list](../../measure/catalog-measure-list-rest-v3.md) method ||
|| **recurSchemeLength**
[`integer`](../../../data-types.md) | Payment period length. Used only in the on-premise version for content sales ||
|| **recurSchemeType**
[`string`](../../../data-types.md) | Payment period time unit. Used only in the on-premise version for content sales ||
|| **trialPriceId**
[`integer`](../../../data-types.md) | Product for trial payment. Used only in the on-premise version for content sales ||
|| **withoutOrder**
[`string`](../../../data-types.md) | Renewal without placing an order: `Y` or `N`. Used only in the on-premise version for content sales ||
|| **quantityTraceRaw**
[`string`](../../../data-types.md) | Quantity tracking, taking the default value into account.

Possible values:

- `Y` — enabled
- `N` — disabled
- `D` — according to the module settings ||
|| **paymentType**
[`string`](../../../data-types.md) | Payment type.

Possible values:

- `S` — one-time
- `R` — recurring
- `T` — trial

Used only in the on-premise version for content sales ||
|| **subscribeRaw**
[`string`](../../../data-types.md) | Product subscription, taking the default value into account.

Possible values:

- `Y` — allowed
- `N` — not allowed
- `D` — according to the module settings ||
|| **canBuyZeroRaw**
[`string`](../../../data-types.md) | Purchase when out of stock, taking the default value into account.

Possible values:

- `Y` — allowed
- `N` — not allowed
- `D` — according to the module settings ||
|| **properties**
[`object`](../../../data-types.md) | Product property values. The key is `PROPERTY_{id}`, where `{id}` is the property identifier. The value can be passed as a string, a number, or a `{"VALUE": ...}` object; for a multiple property, as an array of such values.

Keys of any other form cause a validation error. One property cannot be passed under two keys, for example `PROPERTY_101` and `101`.

To link the variation to a parent product, pass its identifier in the binding property, for example `{"PROPERTY_101": 7553}`. The binding property identifier is the `skuPropertyId` field of the offers catalog, returned by the [catalog.catalog.get](../../catalog/catalog-catalog-get-rest-v3.md) method. The parent product must be a simple product or a parent product from the product information block of this catalog. If the binding property is not passed, a variation without a parent product is created ||
|#

The `id`, `type`, `available`, `bundle`, `dateCreate`, and `timestampX` fields are filled in automatically. If you pass `id` or `type` in `fields`, the method returns an error.

{% note warning "" %}

The method accepts the `previewPicture` and `detailPicture` fields in the `{"fileData": ["file_name", "base64_content"]}` and `{"remove": "Y"}` formats but currently does not save them: the picture is neither added nor removed, and no error occurs. Add product images using the [catalog.product.image.add](../../product-image/catalog-product-image-add-rest-v3.md) method.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.offer.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":27,"name":"DOCS-V3-OFFER Blue T-shirt M","code":"docs-v3-offer-blue-m","measure":9,"vatId":1,"vatIncluded":"Y","weight":200,"properties":{"PROPERTY_101":7553}}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.offer.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":27,"name":"DOCS-V3-OFFER Blue T-shirt M","code":"docs-v3-offer-blue-m","measure":9,"vatId":1,"vatIncluded":"Y","weight":200,"properties":{"PROPERTY_101":7553}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.offer.add
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type OfferAddResult = {
      item: {
        id: number
        iblockId: number
        name: string
        type: number
        properties: Record<string, unknown>
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<OfferAddResult>({
        method: 'catalog.product.offer.add',
        params: {
          fields: {
            iblockId: 27,
            name: 'DOCS-V3-OFFER Blue T-shirt M',
            code: 'docs-v3-offer-blue-m',
            measure: 9,
            vatId: 1,
            vatIncluded: 'Y',
            weight: 200,
            properties: {
              PROPERTY_101: 7553,
            },
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created:', result.item.id, result.item.type)
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
      async function addOffer() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.offer.add',
            params: {
              fields: {
                iblockId: 27,
                name: 'DOCS-V3-OFFER Blue T-shirt M',
                code: 'docs-v3-offer-blue-m',
                measure: 9,
                vatId: 1,
                vatIncluded: 'Y',
                weight: 200,
                properties: {
                  PROPERTY_101: 7553,
                },
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Created:', result.item.id, result.item.type)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addOffer)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.offer.add(
            fields={
                "iblockId": 27,
                "name": "DOCS-V3-OFFER Blue T-shirt M",
                "code": "docs-v3-offer-blue-m",
                "measure": 9,
                "vatId": 1,
                "vatIncluded": "Y",
                "weight": 200,
                "properties": {
                    "PROPERTY_101": 7553,
                },
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
                'catalog.product.offer.add',
                [
                    'fields' => [
                        'iblockId' => 27,
                        'name' => 'DOCS-V3-OFFER Blue T-shirt M',
                        'code' => 'docs-v3-offer-blue-m',
                        'measure' => 9,
                        'vatId' => 1,
                        'vatIncluded' => 'Y',
                        'weight' => 200,
                        'properties' => [
                            'PROPERTY_101' => 7553,
                        ],
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
        'catalog.product.offer.add',
        {
            fields: {
                iblockId: 27,
                name: 'DOCS-V3-OFFER Blue T-shirt M',
                code: 'docs-v3-offer-blue-m',
                measure: 9,
                vatId: 1,
                vatIncluded: 'Y',
                weight: 200,
                properties: {
                    PROPERTY_101: 7553,
                },
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
        'catalog.product.offer.add',
        [
            'fields' => [
                'iblockId' => 27,
                'name' => 'DOCS-V3-OFFER Blue T-shirt M',
                'code' => 'docs-v3-offer-blue-m',
                'measure' => 9,
                'vatId' => 1,
                'vatIncluded' => 'Y',
                'weight' => 200,
                'properties' => [
                    'PROPERTY_101' => 7553,
                ],
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
    res, err := client.Core().Call(ctx, "catalog.product.offer.add", b24.Params{
    	"fields": b24.Params{
    		"iblockId": 27,
    		"name": "DOCS-V3-OFFER Blue T-shirt M",
    		"code": "docs-v3-offer-blue-m",
    		"measure": 9,
    		"vatId": 1,
    		"vatIncluded": "Y",
    		"weight": 200,
    		"properties": b24.Params{
    			"PROPERTY_101": 7553,
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.product.offer.add: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID   int    `json:"id"`
    	Name string `json:"name"`
    	Type int    `json:"type"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
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
            "id": 7555,
            "iblockId": 27,
            "name": "DOCS-V3-OFFER Blue T-shirt M",
            "code": "docs-v3-offer-blue-m",
            "xmlId": "7555",
            "active": "Y",
            "sort": 500,
            "createdBy": 1,
            "modifiedBy": 1,
            "dateCreate": "2026-10-22T12:18:58+02:00",
            "timestampX": "2026-10-22T12:18:58+02:00",
            "dateActiveFrom": null,
            "dateActiveTo": null,
            "previewText": null,
            "previewTextType": "text",
            "previewPicture": null,
            "detailText": null,
            "detailTextType": "text",
            "detailPicture": null,
            "iblockSectionId": null,
            "iblockSection": [],
            "type": 4,
            "available": "Y",
            "bundle": "N",
            "quantity": 0,
            "quantityReserved": 0,
            "quantityTrace": "Y",
            "canBuyZero": "Y",
            "subscribe": "Y",
            "vatId": 1,
            "vatIncluded": "Y",
            "purchasingPrice": null,
            "purchasingCurrency": null,
            "barcodeMulti": "N",
            "weight": 200,
            "length": null,
            "width": null,
            "height": null,
            "measure": 9,
            "recurSchemeLength": null,
            "recurSchemeType": "D",
            "trialPriceId": null,
            "withoutOrder": "N",
            "quantityTraceRaw": "D",
            "paymentType": "S",
            "subscribeRaw": "D",
            "canBuyZeroRaw": "D",
            "properties": {
                "PROPERTY_101": {
                    "VALUE": "7553",
                    "VALUE_ID": "4019"
                },
                "PROPERTY_907": null,
                "PROPERTY_905": null,
                "PROPERTY_429": null,
                "PROPERTY_891": null,
                "PROPERTY_895": null,
                "PROPERTY_909": null,
                "PROPERTY_911": null,
                "PROPERTY_925": null,
                "PROPERTY_937": null
            }
        }
    },
    "time": {
        "start": 1792660738,
        "finish": 1792660738.516739,
        "duration": 0.5167388916015625,
        "processing": 0,
        "date_start": "2026-10-22T12:18:58+02:00",
        "date_finish": "2026-10-22T12:18:58+02:00",
        "operating_reset_at": 1792660980,
        "operating": 0.12495088577270508
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
[`object`](../../../data-types.md) | Fields of the created variation. [Field description](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Variation identifier ||
|| **iblockId**
[`integer`](../../../data-types.md) | Identifier of the offers catalog information block ||
|| **name**
[`string`](../../../data-types.md) | Variation name ||
|| **code**
[`string`](../../../data-types.md) or `null` | Symbolic code ||
|| **xmlId**
[`string`](../../../data-types.md) | External code. If not set, it matches `id` ||
|| **active**
[`string`](../../../data-types.md) | Activity flag: `Y` — active, `N` — inactive ||
|| **sort**
[`integer`](../../../data-types.md) | Sort order ||
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
[`integer`](../../../data-types.md) or `null` | Identifier of the main section ||
|| **iblockSection**
[`array`](../../../data-types.md) | Array of identifiers of all sections the product is linked to ||
|| **type**
[`integer`](../../../data-types.md) | Product type: `4` — variation with a parent product, `5` — variation without a parent product ||
|| **available**
[`string`](../../../data-types.md) | Availability for purchase: `Y` — available, `N` — unavailable ||
|| **bundle**
[`string`](../../../data-types.md) | Whether the product has a set: `Y` — yes, `N` — no ||
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
[`double`](../../../data-types.md) or `null` | Purchase price. Returned if the user has the "View purchase price" permission ||
|| **purchasingCurrency**
[`string`](../../../data-types.md) or `null` | Purchase price currency. Returned if the user has the "View purchase price" permission ||
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
[`string`](../../../data-types.md) | Payment period time unit. Used only in the on-premise version for content sales ||
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

A variation with a parent product gets type `4`, and a variation without a parent product gets type `5`. A parent product without variations changes its type from `6` to `3` after the first variation is added.

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION",
        "message": "Error validating object.",
        "validation": [
            {
                "message": "Required field \"name\" is empty.",
                "field": "name"
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
|| `iblockId` | Required field `iblockId` is empty | Pass `iblockId` in `fields` ||
|| `name` | Required field `name` is empty | Pass `name` in `fields` ||
|| `id` | Field `id` is read-only | Remove `id` from `fields` ||
|| `type` | Field `type` is read-only | Remove `type` from `fields`: the type is set automatically ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

In some errors, the `field` field contains a numeric error code instead of a field name.

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `iblockId` | This request requires data type `int` in field `iblockId` | Pass a number ||
|| `200040300000` | Iblock Not Found | Pass the identifier of an existing catalog information block ||
|| `-` | productType is not allowed for this catalog | Pass the identifier of the offers catalog information block ||
|| `-` | VAT vatId is not exists | Pass the identifier of an existing VAT rate ||
|| `-` | User createdBy is not exists | Pass the identifier of an existing user. A similar error is returned for `modifiedBy` ||
|| `-` | Currency purchasingCurrency is not exists | Pass the code of an existing currency ||
|| `-` | The properties field accepts only the property keys PROPERTY_{id}, unknown keys: #KEYS# | Pass only keys of the `PROPERTY_{id}` form in `properties` ||
|| `-` | The properties field names one property by several keys: #KEYS# | Pass each property under one key ||
|| `200040500010` | Parent product not found. | Pass the identifier of an existing parent product in the binding property ||
|| `200040500020` | Invalid parent product type. | Only a simple product or a parent product can be the parent product. A variation or a service cannot be specified ||
|| `200040500020` | Invalid information block of the parent product. | Pass a parent product from the product information block that the offers catalog is linked to ||
|| `-` | Parent product must be a single value. | Pass a single identifier in the binding property ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `ProductDto` | Pass only the fields from the [Parameter fields](#fields) table in `fields` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions for the catalog and the information block. The error also occurs if the section from `iblockSectionId` does not exist ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-offer-update-rest-v3.md)
- [{#T}](./catalog-product-offer-get-rest-v3.md)
- [{#T}](./catalog-product-offer-list-rest-v3.md)
- [{#T}](./catalog-product-offer-delete-rest-v3.md)
- [{#T}](./catalog-product-offer-download-rest-v3.md)
- [{#T}](./catalog-product-offer-field-list.md)
- [{#T}](./catalog-product-offer-field-get.md)
