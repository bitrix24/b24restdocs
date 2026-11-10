# Update Product Variation Fields catalog.product.offer.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Edit product" permission, permission to modify the variation in the information block, and one of the permissions: "View product catalog", "View catalog products", or "Edit selling price"

The method `catalog.product.offer.update` modifies the fields of a product variation and returns all its fields after the change.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_product_offer.id`](../../data-types.md#catalog_product_offer) | Identifier of the variation.

You can retrieve variation identifiers using the [catalog.product.offer.list](./catalog-product-offer-list.md) method ||
|| **fields***
[`object`](../../../data-types.md) | Values of the fields to change ([detailed description](#fields)).

The method changes only the passed fields and properties, and the rest keep their previous values. The method accepts an empty object `{}` without an error and returns the variation unchanged ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **iblockId**
[`catalog_catalog.iblockId`](../../data-types.md#catalog_catalog) | Identifier of the variations catalog information block. You do not need to pass it: a variation cannot be moved to another information block ||
|| **name**
[`string`](../../../data-types.md) | Variation name. An empty string is not accepted ||
|| **parentId**
[`catalog_product_sku.id`](../../data-types.md#catalog_product_sku) | Identifier of the parent product. The parent product cannot be changed with this method: the method skips `parentId` without an error, and the link remains the same ||
|| **active**
[`string`](../../../data-types.md) | Activity. Possible values:
- `Y` — the variation is active
- `N` — the variation is inactive

The method saves any other value as `N` ||
|| **code**
[`string`](../../../data-types.md) | Symbolic code ||
|| **xmlId**
[`string`](../../../data-types.md) | External code ||
|| **sort**
[`integer`](../../../data-types.md) | Sorting. The method saves a non-numeric value as `null` ||
|| **modifiedBy**
[`user.id`](../../../data-types.md) | Identifier of the user who modified the variation. The method checks that the user exists ||
|| **iblockSectionId**
[`catalog_section.id`](../../data-types.md#catalog_section) | Identifier of the main section. You can retrieve the identifier using the [catalog.section.list](../../section/catalog-section-list.md) method.

For a nonexistent section, the method returns the `200040300050` access error ||
|| **previewText**
[`string`](../../../data-types.md) | Preview description ||
|| **previewTextType**
[`string`](../../../data-types.md) | Type of the preview description: `text` or `html` ||
|| **detailText**
[`string`](../../../data-types.md) | Detailed description ||
|| **detailTextType**
[`string`](../../../data-types.md) | Type of the detailed description: `text` or `html` ||
|| **previewPicture**
[`object`](../../../data-types.md) | Preview picture in the format `{"fileData": ["file_name.png", "base64_content"]}`. To delete the picture, pass `{"remove": "Y"}` ||
|| **detailPicture**
[`object`](../../../data-types.md) | Detailed picture in the format `{"fileData": ["file_name.png", "base64_content"]}`. To delete the picture, pass `{"remove": "Y"}` ||
|| **measure**
[`catalog_measure.id`](../../data-types.md#catalog_measure) | Identifier of the unit of measurement ||
|| **vatId**
[`catalog_vat.id`](../../data-types.md#catalog_vat) | VAT rate identifier. The method checks that the rate exists ||
|| **vatIncluded**
[`string`](../../../data-types.md) | Whether VAT is included in the price: `Y` or `N`. The method saves any other value as `N` ||
|| **purchasingPrice**
[`double`](../../../data-types.md) | Purchasing price ||
|| **purchasingCurrency**
[`string`](../../../data-types.md) | Purchasing price currency, for example `EUR`. The method checks that the currency exists ||
|| **quantity**
[`double`](../../../data-types.md) | Available quantity ||
|| **quantityReserved**
[`double`](../../../data-types.md) | Reserved quantity ||
|| **quantityTrace**
[`string`](../../../data-types.md) | Quantity accounting: `Y` — enabled, `N` — disabled, `D` — according to the module settings ||
|| **canBuyZero**
[`string`](../../../data-types.md) | Whether purchase is allowed when the product is out of stock: `Y`, `N`, or `D` — according to the module settings ||
|| **subscribe**
[`string`](../../../data-types.md) | Whether subscription to the product is allowed: `Y`, `N`, or `D` — according to the module settings ||
|| **barcodeMulti**
[`string`](../../../data-types.md) | Unique barcode for each item: `Y` or `N` ||
|| **weight**
[`double`](../../../data-types.md) | Weight ||
|| **length**
[`double`](../../../data-types.md) | Length ||
|| **width**
[`double`](../../../data-types.md) | Width ||
|| **height**
[`double`](../../../data-types.md) | Height ||
|| **recurSchemeLength**
[`integer`](../../../data-types.md) | Length of the payment period. Used only in the on-premise version for selling content ||
|| **recurSchemeType**
[`string`](../../../data-types.md) | Time unit of the payment period: `H`, `D`, `W`, `M`, `Q`, `S`, or `Y`. Used only in the on-premise version for selling content ||
|| **trialPriceId**
[`integer`](../../../data-types.md) | Product for trial payment. Used only in the on-premise version for selling content ||
|| **withoutOrder**
[`string`](../../../data-types.md) | Renewal without placing an order: `Y` or `N`. Used only in the on-premise version for selling content ||
|| **propertyN**
[`object`](../../../data-types.md) | Value of a variation property, where `N` is the property identifier, for example `property907`.

Pass the value as an object `{"value": value}`, and for a multiple property as an array of such objects. If you pass the value as a number or a string without an object, the method clears the property. Properties that are not in `fields` keep their previous values ||
|#

The method skips the `id`, `type`, `available`, `bundle`, and `timestampX` fields in `fields` without an error. The method also skips unknown fields.

{% note warning "" %}

Do not pass the `dateCreate`, `dateActiveFrom`, and `dateActiveTo` fields. Currently the method replaces the creation date with `2001-11-30T00:00:00` for any `dateCreate` value and rejects activity dates in any format.

{% endnote %}

The method changes the `quantity` and `purchasingPrice` fields even when inventory management is enabled. To keep warehouse stock and the purchasing price consistent with documents under inventory management, use the [catalog.document.*](../../document/index.md) methods. Change the variation price using the [catalog.price.*](../../price/index.md) methods.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

The example changes the purchasing price, quantity, preview description, and color in the `property891` property of the variation `7695`. Other fields and properties, for example the size in `property907`, keep their previous values.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7695,"fields":{"purchasingPrice":850,"quantity":30,"previewText":"100% cotton, density 180 g/m²","property891":{"value":"Dark red"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.offer.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7695,"fields":{"purchasingPrice":850,"quantity":30,"previewText":"100% cotton, density 180 g/m²","property891":{"value":"Dark red"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.offer.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type OfferProperty = {
      value: string
      valueId: string
      valueEnum?: string
    }

    type OfferFile = {
      id: string
      url: string
      urlMachine: string
    }

    // Shape of the offer returned in result (match the "response handling" section of the page)
    type Offer = {
      id: number
      iblockId: number
      name: string
      active: string
      code: string | null
      xmlId: string
      sort: number | null
      type: number
      available: string
      parentId: OfferProperty | null
      quantity: number | null
      purchasingPrice?: string | null
      purchasingCurrency?: string | null
      previewPicture: OfferFile | null
      detailPicture: OfferFile | null
      dateCreate: ISODate
      timestampX: ISODate
      [key: `property${number}`]: OfferProperty | OfferProperty[] | null
    }

    try {
      const response = await $b24.actions.v2.call.make<{ offer: Offer }>({
        method: 'catalog.product.offer.update',
        params: {
          id: 7695,
          fields: {
            purchasingPrice: 850,
            quantity: 30,
            previewText: '100% cotton, density 180 g/m²',
            property891: {
              value: 'Dark red',
            },
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Updated offer:', result.offer.id, result.offer.timestampX)
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
      async function updateOffer() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.product.offer.update',
            params: {
                  id: 7695,
                  fields: {
                    purchasingPrice: 850,
                    quantity: 30,
                    previewText: '100% cotton, density 180 g/m²',
                    property891: {
                      value: 'Dark red',
                    },
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
          console.info('Updated offer:', result.offer.id, result.offer.timestampX)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateOffer)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.offer.update(
            bitrix_id=7695,
            fields={
                "purchasingPrice": 850,
                "quantity": 30,
                "previewText": "100% cotton, density 180 g/m²",
                "property891": {
                    "value": "Dark red",
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

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.product.offer.update',
                [
                    'id'     => 7695,
                    'fields' => [
                        'purchasingPrice' => 850,
                        'quantity'        => 30,
                        'previewText'     => '100% cotton, density 180 g/m²',
                        'property891'     => [
                            'value' => 'Dark red',
                        ],
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
        'catalog.product.offer.update',
        {
            id: 7695,
            fields: {
                purchasingPrice: 850,
                quantity: 30,
                previewText: '100% cotton, density 180 g/m²',
                property891: {
                    value: 'Dark red',
                },
            },
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
        'catalog.product.offer.update',
        [
            'id' => 7695,
            'fields' => [
                'purchasingPrice' => 850,
                'quantity' => 30,
                'previewText' => '100% cotton, density 180 g/m²',
                'property891' => [
                    'value' => 'Dark red',
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
    res, err := client.Core().Call(ctx, "catalog.product.offer.update", b24.Params{
    	"id":     7695,
    	"fields": b24.Params{
    		"purchasingPrice": 850,
    		"quantity":        30,
    		"previewText":     "100% cotton, density 180 g/m²",
    		"property891":     b24.Params{
    			"value": "Dark red",
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.product.offer.update: %w", err)
    }

    // The method wraps the response in an object with the "offer" key.
    raw, ok := b24.Unwrap(res.Result, "offer")
    if !ok {
    	return fmt.Errorf("the response has no offer key")
    }

    // Optional fields can be null, so pointers are used for them.
    var item struct {
    	ID              b24.ID   `json:"id"`
    	IblockID        int      `json:"iblockId"`
    	Name            string   `json:"name"`
    	Active          string   `json:"active"`
    	Code            *string  `json:"code"`
    	Sort            *int     `json:"sort"`
    	Type            int      `json:"type"`
    	Quantity        *float64 `json:"quantity"`
    	PurchasingPrice *string  `json:"purchasingPrice"`
    	ParentID        *struct {
    		Value   string `json:"value"`
    		ValueID string `json:"valueId"`
    	} `json:"parentId"`
    	PreviewPicture *struct {
    		ID  string `json:"id"`
    		URL string `json:"url"`
    	} `json:"previewPicture"`
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
        "offer": {
            "active": "Y",
            "available": "Y",
            "barcodeMulti": "N",
            "bundle": "N",
            "canBuyZero": "Y",
            "code": "tshirt-red-m",
            "createdBy": 1269,
            "dateActiveFrom": null,
            "dateActiveTo": null,
            "dateCreate": "2026-11-05T19:19:32+01:00",
            "detailPicture": null,
            "detailText": null,
            "detailTextType": "text",
            "height": 20,
            "iblockId": 27,
            "iblockSection": null,
            "iblockSectionId": null,
            "id": 7695,
            "length": 700,
            "measure": 9,
            "modifiedBy": 1269,
            "name": "Red T-shirt, M",
            "parentId": {
                "value": "7603",
                "valueId": "4149"
            },
            "previewPicture": {
                "id": "40057",
                "url": "/rest/catalog.product.download?fields%5BfieldName%5D=previewPicture&fields%5BfileId%5D=40057&fields%5BproductId%5D=7695",
                "urlMachine": "/rest/catalog.product.download?fields%5BfieldName%5D=previewPicture&fields%5BfileId%5D=40057&fields%5BproductId%5D=7695"
            },
            "previewText": "100% cotton, density 180 g/m²",
            "previewTextType": "text",
            "property429": null,
            "property891": {
                "value": "Dark red",
                "valueId": "4155"
            },
            "property895": null,
            "property905": null,
            "property907": {
                "value": "1603",
                "valueEnum": "M",
                "valueId": "4157"
            },
            "property909": null,
            "property911": null,
            "property925": null,
            "property937": null,
            "purchasingCurrency": "EUR",
            "purchasingPrice": "850.00000000",
            "quantity": 30,
            "quantityReserved": null,
            "quantityTrace": "Y",
            "recurSchemeLength": null,
            "recurSchemeType": "D",
            "sort": 100,
            "subscribe": "Y",
            "timestampX": "2026-11-05T19:19:32+01:00",
            "trialPriceId": null,
            "type": 4,
            "vatId": 3,
            "vatIncluded": "Y",
            "weight": 200,
            "width": 500,
            "withoutOrder": "N",
            "xmlId": "tshirt-red-m"
        }
    },
    "time": {
        "start": 1793895572,
        "finish": 1793895572.996419,
        "duration": 0.9964189529418945,
        "processing": 0,
        "date_start": "2026-11-05T19:19:32+01:00",
        "date_finish": "2026-11-05T19:19:32+01:00",
        "operating_reset_at": 1793896140,
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
|| **result.offer**
[`catalog_product_offer`](../../data-types.md#catalog_product_offer) | Object with all fields and properties of the variation after the change ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "offer does not exist."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user has none of the "View product catalog", "View catalog products", or "Edit selling price" permissions ||
|| `400` | `200040300040` | Access Denied | The user does not have the "Edit product" permission ||
|| `400` | `200040300043` | Access Denied | The user does not have permission to modify this variation in the information block ||
|| `400` | `200040300050` | Access Denied | The section from `iblockSectionId` does not exist, or the user does not have permission to link elements to it ||
|| `400` | Empty value | offer does not exist. | No variation exists with the specified `id`. The method returns the same error if a non-numeric value is passed in `id` ||
|| `400` | `100` | Could not find value for parameter {id} | The `id` parameter was not provided ||
|| `400` | `100` | Could not find value for parameter {fields} | The `fields` parameter was not provided ||
|| `400` | `0` | productType is not allowed for this catalog | The identifier of a product rather than a variation was passed in `id`, or the product information block was passed in `iblockId` ||
|| `400` | `0` | Name is not specified.<br> | An empty string was passed in `name` ||
|| `400` | `0` | Wrong format of field `name`. | An object or an array was passed in the field instead of a value. The text contains the field name ||
|| `400` | `0` | VAT vatId is not exists | The VAT rate from `vatId` was not found ||
|| `400` | `0` | Currency purchasingCurrency is not exists | The currency from `purchasingCurrency` was not found ||
|| `400` | `0` | User modifiedBy is not exists | The user from `modifiedBy` was not found ||
|| `400` | `0` | Incorrect starting date format.<br> | The `dateActiveFrom` field was passed. The method rejects the date in any format ||
|| `400` | `0` | Incorrect expiration date format.<br> | The `dateActiveTo` field was passed. The method rejects the date in any format ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-offer-add.md)
- [{#T}](./catalog-product-offer-get.md)
- [{#T}](./catalog-product-offer-list.md)
- [{#T}](./catalog-product-offer-download.md)
- [{#T}](./catalog-product-offer-delete.md)
- [{#T}](./catalog-product-offer-get-fields-by-filter.md)
