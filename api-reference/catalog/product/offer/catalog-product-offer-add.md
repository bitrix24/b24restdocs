# Add Product Variation catalog.product.offer.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Create product" permission, permission to modify elements of the variations information block, and one of the permissions: "View product catalog", "View catalog products", or "Edit selling price"

The method `catalog.product.offer.add` creates a product variation in the variations catalog.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Field values of the new variation ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **iblockId***
[`catalog_catalog.iblockId`](../../data-types.md#catalog_catalog) | Identifier of the variations catalog information block.

You can retrieve the identifier using the [catalog.catalog.list](../../catalog/catalog-catalog-list.md) method: the variations catalog has the `productIblockId` field filled in ||
|| **name***
[`string`](../../../data-types.md) | Variation name ||
|| **parentId**
[`catalog_product_sku.id`](../../data-types.md#catalog_product_sku) | Identifier of the parent product. The parent can be a simple product or a parent product from the product information block that the variations catalog is linked to.

You can retrieve the identifiers using the [catalog.product.sku.list](../sku/catalog-product-sku-list.md) and [catalog.product.list](../catalog-product-list.md) methods.

If `parentId` is not passed or equals `0`, the method creates a variation without a parent product with type `5`. Such a variation cannot be purchased ||
|| **active**
[`string`](../../../data-types.md) | Activity. Possible values:
- `Y` — the variation is active
- `N` — the variation is inactive

The method saves any other value as `N`.

Default: `Y` ||
|| **code**
[`string`](../../../data-types.md) | Symbolic code. The method does not check that the code is unique ||
|| **xmlId**
[`string`](../../../data-types.md) | External code. If not passed, it matches the variation identifier ||
|| **sort**
[`integer`](../../../data-types.md) | Sorting. The method saves a non-numeric value as `null`.

Default: `500` ||
|| **createdBy**
[`user.id`](../../../data-types.md) | Identifier of the user who created the variation. The method checks that the user exists.

Default: the current user ||
|| **modifiedBy**
[`user.id`](../../../data-types.md) | Identifier of the user who modified the variation. The method checks that the user exists.

Default: the current user ||
|| **iblockSectionId**
[`catalog_section.id`](../../data-types.md#catalog_section) | Identifier of the main section. You can retrieve the identifier using the [catalog.section.list](../../section/catalog-section-list.md) method.

For a nonexistent section, the method returns the `200040300050` access error ||
|| **previewText**
[`string`](../../../data-types.md) | Preview description ||
|| **previewTextType**
[`string`](../../../data-types.md) | Type of the preview description. Possible values:
- `text` — text
- `html` — HTML

Default: `text` ||
|| **detailText**
[`string`](../../../data-types.md) | Detailed description ||
|| **detailTextType**
[`string`](../../../data-types.md) | Type of the detailed description. Possible values:
- `text` — text
- `html` — HTML

Default: `text` ||
|| **previewPicture**
[`object`](../../../data-types.md) | Preview picture in the format `{"fileData": ["file_name.png", "base64_content"]}` ||
|| **detailPicture**
[`object`](../../../data-types.md) | Detailed picture in the format `{"fileData": ["file_name.png", "base64_content"]}` ||
|| **measure**
[`catalog_measure.id`](../../data-types.md#catalog_measure) | Identifier of the unit of measurement. You can retrieve the identifier using the [catalog.measure.list](../../measure/catalog-measure-list.md) method ||
|| **vatId**
[`catalog_vat.id`](../../data-types.md#catalog_vat) | VAT rate identifier. You can retrieve the identifier using the [catalog.vat.list](../../vat/catalog-vat-list.md) method. The method checks that the rate exists ||
|| **vatIncluded**
[`string`](../../../data-types.md) | Whether VAT is included in the price. Possible values:
- `Y` — yes
- `N` — no

The method saves any other value as `N`.

Default: `N` ||
|| **purchasingPrice**
[`double`](../../../data-types.md) | Purchasing price ||
|| **purchasingCurrency**
[`string`](../../../data-types.md) | Purchasing price currency, for example `EUR`. You can retrieve the list of currencies using the [crm.currency.list](../../../crm/currency/crm-currency-list.md) method. The method checks that the currency exists ||
|| **quantity**
[`double`](../../../data-types.md) | Available quantity. The method saves a non-numeric value as `null` ||
|| **quantityReserved**
[`double`](../../../data-types.md) | Reserved quantity ||
|| **quantityTrace**
[`string`](../../../data-types.md) | Quantity accounting. Possible values:
- `Y` — enabled
- `N` — disabled
- `D` — according to the module settings

Default: `D`. The response returns the resulting value `Y` or `N` ||
|| **canBuyZero**
[`string`](../../../data-types.md) | Whether purchase is allowed when the product is out of stock. Possible values:
- `Y` — yes
- `N` — no
- `D` — according to the module settings

Default: `D`. The response returns the resulting value `Y` or `N` ||
|| **subscribe**
[`string`](../../../data-types.md) | Whether subscription to the product is allowed. Possible values:
- `Y` — yes
- `N` — no
- `D` — according to the module settings

Default: `D`. The response returns the resulting value `Y` or `N` ||
|| **barcodeMulti**
[`string`](../../../data-types.md) | Unique barcode for each item. Possible values:
- `Y` — yes
- `N` — no

Default: `N` ||
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
[`string`](../../../data-types.md) | Time unit of the payment period. Possible values:
- `H` — hour
- `D` — day
- `W` — week
- `M` — month
- `Q` — quarter
- `S` — half-year
- `Y` — year

Default: `D`. Used only in the on-premise version for selling content ||
|| **trialPriceId**
[`integer`](../../../data-types.md) | Product for trial payment. Used only in the on-premise version for selling content ||
|| **withoutOrder**
[`string`](../../../data-types.md) | Renewal without placing an order. Possible values:
- `Y` — yes
- `N` — no

Default: `N`. Used only in the on-premise version for selling content ||
|| **propertyN**
[`any`](../../../data-types.md) | Value of a variation property, where `N` is the property identifier, for example `property907`. You can retrieve the properties of the variations catalog using the [catalog.product.offer.getFieldsByFilter](./catalog-product-offer-get-fields-by-filter.md) method.

Pass the value as a number, a string, or an object `{"value": value}`. For a list property, pass the identifier of the value option. For a multiple property, pass an array of values ||
|#

The method fills in the `id`, `type`, `available`, `bundle`, and `timestampX` fields itself and skips them in `fields` without an error. The method also skips unknown fields.

{% note warning "" %}

Do not pass the `dateCreate`, `dateActiveFrom`, and `dateActiveTo` fields. Currently the method saves any `dateCreate` value as `2001-11-30T00:00:00` and rejects activity dates in any format with the `Incorrect starting date format.<br>` or `Incorrect expiration date format.<br>` error.

{% endnote %}

Set the variation price using the [catalog.price.*](../../price/index.md) methods after creating the variation. The method saves the `quantity` and `purchasingPrice` fields even when inventory management is enabled. To keep warehouse stock and the purchasing price consistent with documents under inventory management, use the [catalog.document.*](../../document/index.md) methods.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

The example creates the "Red T-shirt, M" variation for the parent product `7603` and sets the size in the `property907` list property, the color in the `property891` string property, and the preview picture. In the example, the picture content is shortened to `iVBORw0KGgo...`, so pass the full base64 string.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":27,"name":"Red T-shirt, M","parentId":7603,"active":"Y","code":"tshirt-red-m","xmlId":"tshirt-red-m","sort":100,"measure":9,"vatId":3,"vatIncluded":"Y","purchasingPrice":800,"purchasingCurrency":"EUR","quantity":25,"weight":200,"width":500,"length":700,"height":20,"previewText":"100% cotton","previewPicture":{"fileData":["tshirt-red.png","iVBORw0KGgo..."]},"property907":1603,"property891":"Red"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.offer.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":27,"name":"Red T-shirt, M","parentId":7603,"active":"Y","code":"tshirt-red-m","xmlId":"tshirt-red-m","sort":100,"measure":9,"vatId":3,"vatIncluded":"Y","purchasingPrice":800,"purchasingCurrency":"EUR","quantity":25,"weight":200,"width":500,"length":700,"height":20,"previewText":"100% cotton","previewPicture":{"fileData":["tshirt-red.png","iVBORw0KGgo..."]},"property907":1603,"property891":"Red"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.offer.add
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
        method: 'catalog.product.offer.add',
        params: {
          fields: {
            iblockId: 27,
            name: 'Red T-shirt, M',
            parentId: 7603,
            active: 'Y',
            code: 'tshirt-red-m',
            xmlId: 'tshirt-red-m',
            sort: 100,
            measure: 9,
            vatId: 3,
            vatIncluded: 'Y',
            purchasingPrice: 800,
            purchasingCurrency: 'EUR',
            quantity: 25,
            weight: 200,
            width: 500,
            length: 700,
            height: 20,
            previewText: '100% cotton',
            previewPicture: {
              fileData: ['tshirt-red.png', 'iVBORw0KGgo...'],
            },
            property907: 1603,
            property891: 'Red',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Added offer:', result.offer.id, result.offer.name)
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
      async function addOffer() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.product.offer.add',
            params: {
                  fields: {
                    iblockId: 27,
                    name: 'Red T-shirt, M',
                    parentId: 7603,
                    active: 'Y',
                    code: 'tshirt-red-m',
                    xmlId: 'tshirt-red-m',
                    sort: 100,
                    measure: 9,
                    vatId: 3,
                    vatIncluded: 'Y',
                    purchasingPrice: 800,
                    purchasingCurrency: 'EUR',
                    quantity: 25,
                    weight: 200,
                    width: 500,
                    length: 700,
                    height: 20,
                    previewText: '100% cotton',
                    previewPicture: {
                      fileData: ['tshirt-red.png', 'iVBORw0KGgo...'],
                    },
                    property907: 1603,
                    property891: 'Red',
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
          console.info('Added offer:', result.offer.id, result.offer.name)
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
                "name": "Red T-shirt, M",
                "parentId": 7603,
                "active": "Y",
                "code": "tshirt-red-m",
                "xmlId": "tshirt-red-m",
                "sort": 100,
                "measure": 9,
                "vatId": 3,
                "vatIncluded": "Y",
                "purchasingPrice": 800,
                "purchasingCurrency": "EUR",
                "quantity": 25,
                "weight": 200,
                "width": 500,
                "length": 700,
                "height": 20,
                "previewText": "100% cotton",
                "previewPicture": {
                    "fileData": ["tshirt-red.png", "iVBORw0KGgo..."],
                },
                "property907": 1603,
                "property891": "Red",
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
                'catalog.product.offer.add',
                [
                    'fields' => [
                        'iblockId'           => 27,
                        'name'               => 'Red T-shirt, M',
                        'parentId'           => 7603,
                        'active'             => 'Y',
                        'code'               => 'tshirt-red-m',
                        'xmlId'              => 'tshirt-red-m',
                        'sort'               => 100,
                        'measure'            => 9,
                        'vatId'              => 3,
                        'vatIncluded'        => 'Y',
                        'purchasingPrice'    => 800,
                        'purchasingCurrency' => 'EUR',
                        'quantity'           => 25,
                        'weight'             => 200,
                        'width'              => 500,
                        'length'             => 700,
                        'height'             => 20,
                        'previewText'        => '100% cotton',
                        'previewPicture'     => [
                            'fileData' => ['tshirt-red.png', 'iVBORw0KGgo...'],
                        ],
                        'property907'        => 1603,
                        'property891'        => 'Red',
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
        'catalog.product.offer.add',
        {
            fields: {
                iblockId: 27,
                name: 'Red T-shirt, M',
                parentId: 7603,
                active: 'Y',
                code: 'tshirt-red-m',
                xmlId: 'tshirt-red-m',
                sort: 100,
                measure: 9,
                vatId: 3,
                vatIncluded: 'Y',
                purchasingPrice: 800,
                purchasingCurrency: 'EUR',
                quantity: 25,
                weight: 200,
                width: 500,
                length: 700,
                height: 20,
                previewText: '100% cotton',
                previewPicture: {
                    fileData: ['tshirt-red.png', 'iVBORw0KGgo...'],
                },
                property907: 1603,
                property891: 'Red',
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
        'catalog.product.offer.add',
        [
            'fields' => [
                'iblockId' => 27,
                'name' => 'Red T-shirt, M',
                'parentId' => 7603,
                'active' => 'Y',
                'code' => 'tshirt-red-m',
                'xmlId' => 'tshirt-red-m',
                'sort' => 100,
                'measure' => 9,
                'vatId' => 3,
                'vatIncluded' => 'Y',
                'purchasingPrice' => 800,
                'purchasingCurrency' => 'EUR',
                'quantity' => 25,
                'weight' => 200,
                'width' => 500,
                'length' => 700,
                'height' => 20,
                'previewText' => '100% cotton',
                'previewPicture' => [
                    'fileData' => ['tshirt-red.png', 'iVBORw0KGgo...'],
                ],
                'property907' => 1603,
                'property891' => 'Red',
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
    		"iblockId":           27,
    		"name":               "Red T-shirt, M",
    		"parentId":           7603,
    		"active":             "Y",
    		"code":               "tshirt-red-m",
    		"xmlId":              "tshirt-red-m",
    		"sort":               100,
    		"measure":            9,
    		"vatId":              3,
    		"vatIncluded":        "Y",
    		"purchasingPrice":    800,
    		"purchasingCurrency": "EUR",
    		"quantity":           25,
    		"weight":             200,
    		"width":              500,
    		"length":             700,
    		"height":             20,
    		"previewText":        "100% cotton",
    		"previewPicture":     b24.Params{
    			"fileData": []string{"tshirt-red.png", "iVBORw0KGgo..."},
    		},
    		"property907":        1603,
    		"property891":        "Red",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.product.offer.add: %w", err)
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
            "previewText": "100% cotton",
            "previewTextType": "text",
            "property429": null,
            "property891": {
                "value": "Red",
                "valueId": "4151"
            },
            "property895": null,
            "property905": null,
            "property907": {
                "value": "1603",
                "valueEnum": "M",
                "valueId": "4153"
            },
            "property909": null,
            "property911": null,
            "property925": null,
            "property937": null,
            "purchasingCurrency": "EUR",
            "purchasingPrice": "800.00000000",
            "quantity": 25,
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
        "start": 1793895571,
        "finish": 1793895572.306547,
        "duration": 1.306546926498413,
        "processing": 1,
        "date_start": "2026-11-05T19:19:31+01:00",
        "date_finish": "2026-11-05T19:19:32+01:00",
        "operating_reset_at": 1793896140,
        "operating": 0.30707502365112305
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
[`catalog_product_offer`](../../data-types.md#catalog_product_offer) | Object with the fields of the created variation. The method returns all fields and properties of the variation, with empty ones as `null`.

A property is returned as an object `{"value": ..., "valueId": ...}`, for a list property with the option text in `valueEnum`. A picture is returned as an object with the file identifier `id` and relative links `url` and `urlMachine` to the [catalog.product.download](../catalog-product-download.md) method. The purchasing price `purchasingPrice` is returned as a string ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "200040500010",
    "error_description": "Parent product not found."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user has none of the "View product catalog", "View catalog products", or "Edit selling price" permissions ||
|| `400` | `200040300040` | Access Denied | The user does not have the "Create product" permission ||
|| `400` | `200040300043` | Access Denied | The user does not have permission to modify elements of the variations information block ||
|| `400` | `200040300050` | Access Denied | The section from `iblockSectionId` does not exist, or the user does not have permission to link elements to it ||
|| `400` | `200040500010` | Parent product not found. | The parent product from `parentId` was not found ||
|| `400` | `200040500020` | Invalid parent product type. | A variation or a service was passed in `parentId`. Only a simple product or a parent product can be the parent ||
|| `400` | `100` | Could not find value for parameter {fields} | The `fields` parameter was not provided ||
|| `400` | `0` | Incorrect fields format | `fields` is not an object ||
|| `400` | `0` | Required fields: name | The `name` field was not provided. The text lists all missing required fields ||
|| `400` | `0` | iblock is not catalog | The `iblockId` field was not provided, or the information block is not a catalog ||
|| `400` | `0` | productType is not allowed for this catalog | The product information block was passed in `iblockId` instead of the variations information block ||
|| `400` | `0` | Wrong format of field `name`. | An object or an array was passed in the field instead of a value. The text contains the field name ||
|| `400` | `0` | VAT vatId is not exists | The VAT rate from `vatId` was not found ||
|| `400` | `0` | Currency purchasingCurrency is not exists | The currency from `purchasingCurrency` was not found ||
|| `400` | `0` | User createdBy is not exists | The user from `createdBy` was not found. For `modifiedBy`, the error text is `User modifiedBy is not exists` ||
|| `400` | `0` | Incorrect starting date format.<br> | The `dateActiveFrom` field was passed. The method rejects the date in any format ||
|| `400` | `0` | Incorrect expiration date format.<br> | The `dateActiveTo` field was passed. The method rejects the date in any format ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-offer-update.md)
- [{#T}](./catalog-product-offer-get.md)
- [{#T}](./catalog-product-offer-list.md)
- [{#T}](./catalog-product-offer-download.md)
- [{#T}](./catalog-product-offer-delete.md)
- [{#T}](./catalog-product-offer-get-fields-by-filter.md)
