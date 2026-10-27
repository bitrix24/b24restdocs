# Add Product v 3.0 catalog.product.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Create product" permission and one of the following permissions: "View product catalog", "View catalog products", or "Edit selling price"

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.product.add` method creates a simple product in the trade catalog and returns its fields.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | An object with the fields of the new product. [Field description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **iblockId***
[`integer`](../../data-types.md) | Identifier of the trade catalog information block.

The identifier can be retrieved using the [catalog.catalog.list](../catalog/catalog-catalog-list-rest-v3.md) method. The information block of the offers catalog is not suitable: the method returns the "Incorrect product type" error ||
|| **name***
[`string`](../../data-types.md) | Product name ||
|| **code**
[`string`](../../data-types.md) | Symbolic code ||
|| **xmlId**
[`string`](../../data-types.md) | External code. If not passed, it matches the product identifier ||
|| **active**
[`string`](../../data-types.md) | Product activity.

Possible values:

- `Y` — active
- `N` — inactive

Other values are saved as `N`.

Default: `Y` ||
|| **sort**
[`integer`](../../data-types.md) | Sort order.

Default: `500` ||
|| **createdBy**
[`integer`](../../data-types.md) | Identifier of the user who created the product. The identifier can be retrieved using the [user.get](../../user/user-get.md) method ||
|| **modifiedBy**
[`integer`](../../data-types.md) | Identifier of the user who modified the product ||
|| **dateActiveFrom**
[`datetime`](../../data-types.md) | Activity start date. Currently, the date cannot be set with REST 3.0 methods: a value in ISO 8601 format causes the "Wrong format of field" error, and values in other formats cause a data type error. Only `null` is accepted ||
|| **dateActiveTo**
[`datetime`](../../data-types.md) | Activity end date. Currently, the date cannot be set with REST 3.0 methods: a value in ISO 8601 format causes the "Wrong format of field" error, and values in other formats cause a data type error. Only `null` is accepted ||
|| **previewText**
[`string`](../../data-types.md) | Preview text ||
|| **previewTextType**
[`string`](../../data-types.md) | Preview text type.

Possible values:

- `text` — text
- `html` — HTML

Default: `text` ||
|| **previewPicture**
[`object`](../../data-types.md) | Preview picture. Currently, the file cannot be uploaded or deleted with REST 3.0 methods: a value in the `{"fileData": ["name.png", "base64"]}` format passes validation, but the file is not saved. Upload the file using the previous version method [catalog.product.update](./catalog-product-update.md) ||
|| **detailText**
[`string`](../../data-types.md) | Detail text ||
|| **detailTextType**
[`string`](../../data-types.md) | Detail text type.

Possible values:

- `text` — text
- `html` — HTML

Default: `text` ||
|| **detailPicture**
[`object`](../../data-types.md) | Detail picture. Currently, the file cannot be uploaded or deleted with REST 3.0 methods: a value in the `{"fileData": ["name.png", "base64"]}` format passes validation, but the file is not saved. Upload the file using the previous version method [catalog.product.update](./catalog-product-update.md) ||
|| **iblockSectionId**
[`integer`](../../data-types.md) | Identifier of the main section. The identifier can be retrieved using the [catalog.section.list](../section/catalog-section-list-rest-v3.md) method ||
|| **iblockSection**
[`array`](../../data-types.md) | An array of identifiers of the sections to link the product to, for example `[41, 39]`. Set the main section in `iblockSectionId` ||
|| **quantity**
[`double`](../../data-types.md) | Available quantity. When inventory management is enabled, the value is not saved: the method does not return an error, but the field keeps its previous value ||
|| **quantityReserved**
[`double`](../../data-types.md) | Reserved quantity. When inventory management is enabled, the value is not saved: the method does not return an error, but the field keeps its previous value ||
|| **quantityTrace**
[`string`](../../data-types.md) | Quantity tracking mode.

Possible values:

- `Y` — enabled
- `N` — disabled
- `D` — the value from the Trade Catalog module settings

When inventory management is enabled, the value is not saved: the method does not return an error, but the field keeps its previous value ||
|| **canBuyZero**
[`string`](../../data-types.md) | Allow purchase when the product is out of stock.

Possible values:

- `Y` — yes
- `N` — no
- `D` — the value from the Trade Catalog module settings ||
|| **subscribe**
[`string`](../../data-types.md) | Allow subscription to the product.

Possible values:

- `Y` — yes
- `N` — no
- `D` — the value from the Trade Catalog module settings ||
|| **vatId**
[`integer`](../../data-types.md) | VAT rate identifier. The identifier can be retrieved using the [catalog.vat.list](../vat/catalog-vat-list-rest-v3.md) method ||
|| **vatIncluded**
[`string`](../../data-types.md) | VAT is included in the price.

Possible values:

- `Y` — yes
- `N` — no ||
|| **purchasingPrice**
[`double`](../../data-types.md) | Purchase price. When inventory management is enabled, the value is not saved: the method does not return an error, but the field keeps its previous value ||
|| **purchasingCurrency**
[`string`](../../data-types.md) | Purchase price currency, for example `EUR`. When inventory management is enabled, the value is not saved: the method does not return an error, but the field keeps its previous value ||
|| **barcodeMulti**
[`string`](../../data-types.md) | Unique barcode for each item.

Possible values:

- `Y` — yes
- `N` — no ||
|| **weight**
[`double`](../../data-types.md) | Weight ||
|| **length**
[`double`](../../data-types.md) | Length ||
|| **width**
[`double`](../../data-types.md) | Width ||
|| **height**
[`double`](../../data-types.md) | Height ||
|| **measure**
[`integer`](../../data-types.md) | Unit of measurement identifier. The identifier can be retrieved using the [catalog.measure.list](../measure/catalog-measure-list-rest-v3.md) method ||
|| **recurSchemeLength**
[`integer`](../../data-types.md) | Payment period length. Used only in the on-premise version for content sales ||
|| **recurSchemeType**
[`string`](../../data-types.md) | Time unit of the payment period. Used only in the on-premise version for content sales ||
|| **trialPriceId**
[`integer`](../../data-types.md) | Identifier of the product for trial payment. Used only in the on-premise version for content sales ||
|| **withoutOrder**
[`string`](../../data-types.md) | Renewal without placing an order. Used only in the on-premise version for content sales ||
|| **quantityTraceRaw**
[`string`](../../data-types.md) | Quantity tracking mode including the module settings: `Y`, `N`, or `D` ||
|| **paymentType**
[`string`](../../data-types.md) | Payment type. Used only in the on-premise version for content sales ||
|| **subscribeRaw**
[`string`](../../data-types.md) | Subscription permission including the module settings: `Y`, `N`, or `D` ||
|| **canBuyZeroRaw**
[`string`](../../data-types.md) | Permission to purchase an out-of-stock product including the module settings: `Y`, `N`, or `D` ||
|| **properties**
[`object`](../../data-types.md) | Product property values. The key is `PROPERTY_{id}`, where `id` is the property identifier, for example `{"PROPERTY_301": "TS-001", "PROPERTY_305": ["Cotton", "Polyester"]}`.

Pass the value of a single property as a string or a number, and the values of a multiple property as an array.

Property identifiers can be retrieved using the [catalog.productProperty.list](../product-property/catalog-product-property-list.md) method. Properties of the "File" type are currently not saved ||
|#

The `id`, `type`, `available`, `bundle`, `dateCreate`, and `timestampX` fields are filled in automatically. If you pass them in `fields`, the method returns an error.

{% note warning "" %}

Currently, REST 3.0 methods do not save product files: the `previewPicture` and `detailPicture` pictures and values of "File" type properties. Upload the files using the previous version method [catalog.product.update](./catalog-product-update.md).

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":25,"name":"Basic T-shirt","code":"tshirt-basic","iblockSectionId":41,"vatId":1,"vatIncluded":"Y","measure":9,"previewText":"Cotton T-shirt","properties":{"PROPERTY_301":"TS-001","PROPERTY_305":["Cotton","Polyester"]}}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":25,"name":"Basic T-shirt","code":"tshirt-basic","iblockSectionId":41,"vatId":1,"vatIncluded":"Y","measure":9,"previewText":"Cotton T-shirt","properties":{"PROPERTY_301":"TS-001","PROPERTY_305":["Cotton","Polyester"]}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.add
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProductResult = {
      item: {
        id: number
        iblockId: number
        name: string
        code: string | null
        active: 'Y' | 'N'
        iblockSectionId: number | null
        type: number
        vatId: number | null
        measure: number | null
        dateCreate: ISODate
        properties: Record<string, { VALUE: string, VALUE_ID: string } | Array<{ VALUE: string, VALUE_ID: string }> | null>
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<ProductResult>({
        method: 'catalog.product.add',
        params: {
          fields: {
            iblockId: 25,
            name: 'Basic T-shirt',
            code: 'tshirt-basic',
            iblockSectionId: 41,
            vatId: 1,
            vatIncluded: 'Y',
            measure: 9,
            previewText: 'Cotton T-shirt',
            properties: {
              PROPERTY_301: 'TS-001',
              PROPERTY_305: ['Cotton', 'Polyester'],
            },
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Product created:', result.item.id)
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
      async function product() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.add',
            params: {
              fields: {
                iblockId: 25,
                name: 'Basic T-shirt',
                code: 'tshirt-basic',
                iblockSectionId: 41,
                vatId: 1,
                vatIncluded: 'Y',
                measure: 9,
                previewText: 'Cotton T-shirt',
                properties: {
                  PROPERTY_301: 'TS-001',
                  PROPERTY_305: ['Cotton', 'Polyester'],
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
          console.info('Product created:', result.item.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', product)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.add(
            fields={
                "iblockId": 25,
                "name": "Basic T-shirt",
                "code": "tshirt-basic",
                "iblockSectionId": 41,
                "vatId": 1,
                "vatIncluded": "Y",
                "measure": 9,
                "previewText": "Cotton T-shirt",
                "properties": {
                    "PROPERTY_301": "TS-001",
                    "PROPERTY_305": ["Cotton", "Polyester"],
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
                'catalog.product.add',
                [
                    'fields' => [
                        'iblockId' => 25,
                        'name' => 'Basic T-shirt',
                        'code' => 'tshirt-basic',
                        'iblockSectionId' => 41,
                        'vatId' => 1,
                        'vatIncluded' => 'Y',
                        'measure' => 9,
                        'previewText' => 'Cotton T-shirt',
                        'properties' => [
                            'PROPERTY_301' => 'TS-001',
                            'PROPERTY_305' => ['Cotton', 'Polyester'],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'catalog.product.add',
        {
            fields: {
                iblockId: 25,
                name: 'Basic T-shirt',
                code: 'tshirt-basic',
                iblockSectionId: 41,
                vatId: 1,
                vatIncluded: 'Y',
                measure: 9,
                previewText: 'Cotton T-shirt',
                properties: {
                    PROPERTY_301: 'TS-001',
                    PROPERTY_305: ['Cotton', 'Polyester'],
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
        'catalog.product.add',
        [
            'fields' => [
                'iblockId' => 25,
                'name' => 'Basic T-shirt',
                'code' => 'tshirt-basic',
                'iblockSectionId' => 41,
                'vatId' => 1,
                'vatIncluded' => 'Y',
                'measure' => 9,
                'previewText' => 'Cotton T-shirt',
                'properties' => [
                    'PROPERTY_301' => 'TS-001',
                    'PROPERTY_305' => ['Cotton', 'Polyester'],
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
    res, err := client.Core().Call(ctx, "catalog.product.add", b24.Params{
    	"fields":b24.Params{
    		"iblockId":       25,
    		"name":           "Basic T-shirt",
    		"code":           "tshirt-basic",
    		"iblockSectionId":41,
    		"vatId":          1,
    		"vatIncluded":    "Y",
    		"measure":        9,
    		"previewText":    "Cotton T-shirt",
    		"properties":     b24.Params{
    			"PROPERTY_301":"TS-001",
    			"PROPERTY_305":[]string{"Cotton", "Polyester"},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.product.add: %w", err)
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
    	Properties map[string]json.RawMessage `json:"properties"`
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
            "name": "Basic T-shirt",
            "code": "tshirt-basic",
            "xmlId": "7549",
            "active": "Y",
            "sort": 500,
            "createdBy": 1,
            "modifiedBy": 1,
            "dateCreate": "2026-10-22T12:17:25+02:00",
            "timestampX": "2026-10-22T12:17:25+02:00",
            "dateActiveFrom": null,
            "dateActiveTo": null,
            "previewText": "Cotton T-shirt",
            "previewTextType": "text",
            "previewPicture": null,
            "detailText": null,
            "detailTextType": "text",
            "detailPicture": null,
            "iblockSectionId": 41,
            "iblockSection": [
                41
            ],
            "type": 1,
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
            "weight": 0,
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
                "PROPERTY_301": {
                    "VALUE": "TS-001",
                    "VALUE_ID": "4003"
                },
                "PROPERTY_303": null,
                "PROPERTY_305": [
                    {
                        "VALUE": "Cotton",
                        "VALUE_ID": "4005"
                    },
                    {
                        "VALUE": "Polyester",
                        "VALUE_ID": "4007"
                    }
                ],
                "PROPERTY_359": null
            }
        }
    },
    "time": {
        "start": 1792660645,
        "finish": 1792660645.871675,
        "duration": 0.8716750144958496,
        "processing": 0,
        "date_start": "2026-10-22T12:17:25+02:00",
        "date_finish": "2026-10-22T12:17:25+02:00",
        "operating_reset_at": 1792660860,
        "operating": 0.38492894172668457
    }
}
```

In the response, `properties` contains all properties of the information block. The example shows some of them.

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | An object with the response data ||
|| **result.item**
[`object`](../../data-types.md) | Fields of the created product. [Field description](#item) ||
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

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `iblockId` | Required field `iblockId` is empty | Pass `iblockId` in `fields` ||
|| `name` | Required field `name` is empty | Pass `name` in `fields` ||
|| `#FIELD#` | Field `#FIELD#` is read-only | Remove the `id`, `type`, `available`, `bundle`, `dateCreate`, `timestampX` fields from `fields` ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `200040300000` | Iblock Not Found | Pass the identifier of an existing trade catalog information block ||
|| `BX_ERROR` | Incorrect product type | Pass an information block of the product catalog, not of the offers catalog ||
|| `-` | VAT vatId is not exists | Pass the identifier of an existing VAT rate ||
|| `-` | Currency purchasingCurrency is not exists | Pass the code of an existing currency ||
|| `-` | User createdBy is not exists | Pass the identifier of an existing user ||
|| `-` | User modifiedBy is not exists | Pass the identifier of an existing user ||
|| `-` | The properties field accepts only the property keys PROPERTY_{id}, unknown keys: #KEYS# | Pass only keys in the `PROPERTY_{id}` format in `properties` ||
|| `-` | The properties field names one property by several keys: #KEYS# | Specify each property with one key, for example only `PROPERTY_301` ||
|| `dateActiveFrom` | Wrong format of field `dateActiveFrom`. | Currently, the date cannot be set with REST 3.0 methods. Pass `null` or do not pass the field ||
|| `dateActiveFrom` | This request requires data type `DateTime` in field `dateActiveFrom` | Currently, the date cannot be set with REST 3.0 methods. Pass `null` or do not pass the field ||
|| `sort` | This request requires data type `int` in field `sort` | Pass an integer ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Unknown field `#FIELD#` found in entity `ProductDto` | Pass only the fields from the [Parameter fields](#fields) table in `fields`. Pass property values in `properties` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "Create product" permission and permission to modify information block elements. The error is also caused by a section in `iblockSectionId` that does not exist in the information block ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-update-rest-v3.md)
- [{#T}](./catalog-product-get-rest-v3.md)
- [{#T}](./catalog-product-list-rest-v3.md)
- [{#T}](./catalog-product-delete-rest-v3.md)
- [{#T}](./catalog-product-field-list.md)
