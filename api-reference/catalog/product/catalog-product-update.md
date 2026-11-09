# Update Product catalog.product.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Edit product" permission and one of the following permissions: "View product catalog", "View catalog products", or "Edit selling price"

The `catalog.product.update` method updates a product in the trade catalog. Fields that are not in the request keep their previous values.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_product.id`](../data-types.md#catalog_product) | Product identifier. You can retrieve the identifier using the [catalog.product.list](./catalog-product-list.md) method ||
|| **fields***
[`object`](../../data-types.md) | Object with the product fields to change [(detailed description)](#fields).

If you pass an empty object, the method returns the product unchanged.

The product information block cannot be changed: if you pass a different `iblockId`, the method returns no error and does not move the product ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Product name. An empty value causes the "Name is not specified" error ||
|| **active**
[`string`](../../data-types.md) | Product activity.

Possible values:

- `Y` — active
- `N` — inactive

Other values are saved as `N` ||
|| **code**
[`string`](../../data-types.md) | Symbolic code ||
|| **xmlId**
[`string`](../../data-types.md) | External code ||
|| **sort**
[`integer`](../../data-types.md) | Sorting ||
|| **createdBy**
[`user.id`](../../data-types.md) | Identifier of the user who created the product. You can retrieve the identifier using the [user.get](../../user/user-get.md) method ||
|| **modifiedBy**
[`user.id`](../../data-types.md) | Identifier of the user who modified the product ||
|| **dateActiveFrom**
[`datetime`](../../data-types.md) | Activity start date. Currently the date cannot be set: a string in any format causes the "Incorrect starting date format" error, and a number is ignored. `null` is accepted ||
|| **dateActiveTo**
[`datetime`](../../data-types.md) | Activity end date. Currently the date cannot be set: a string in any format causes the "Incorrect expiration date format" error, and a number is ignored. `null` is accepted ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Creation date. The field is filled in automatically, so you do not need to pass it: currently a passed date is saved incorrectly, for example, `2024-05-27T10:00:00+01:00` is saved as `2001-11-30T00:00:00+01:00` ||
|| **iblockSectionId**
[`catalog_section.id`](../data-types.md#catalog_section) | Identifier of the main section. You can retrieve the identifier using the [catalog.section.list](../section/catalog-section-list.md) method.

For a nonexistent section, the method returns the `200040300050` error. To unlink the product from sections, pass `0` ||
|| **iblockSection**
[`catalog_section.id[]`](../data-types.md#catalog_section) | Array of identifiers of the sections to link the product to, for example, `[41, 39]`. Set the main section in `iblockSectionId`.

The array replaces the current section links ||
|| **previewText**
[`string`](../../data-types.md) | Preview description ||
|| **previewTextType**
[`string`](../../data-types.md) | Type of the preview description.

Possible values:

- `text` — text
- `html` — HTML ||
|| **previewPicture**
[`object`](../../data-types.md) | Preview picture. An object in the format `{"fileData": ["photo.png", "base64"]}`: the first element is the file name with the extension, the second is the file content in [Base64](../../files/how-to-upload-files.md) encoding.

To delete the picture, pass `{"remove": "Y"}` ||
|| **detailText**
[`string`](../../data-types.md) | Detailed description ||
|| **detailTextType**
[`string`](../../data-types.md) | Type of the detailed description.

Possible values:

- `text` — text
- `html` — HTML ||
|| **detailPicture**
[`object`](../../data-types.md) | Detailed picture. An object in the format `{"fileData": ["photo.png", "base64"]}`: the first element is the file name with the extension, the second is the file content in [Base64](../../files/how-to-upload-files.md) encoding.

To delete the picture, pass `{"remove": "Y"}` ||
|| **quantity**
[`double`](../../data-types.md) | Available quantity. When inventory management is enabled, the value is not retained: the method returns no error, but the field keeps its previous value ||
|| **quantityReserved**
[`double`](../../data-types.md) | Reserved quantity. When inventory management is enabled, the value is not retained: the method returns no error, but the field keeps its previous value ||
|| **quantityTrace**
[`string`](../../data-types.md) | Quantity accounting mode.

Possible values:

- `Y` — enabled
- `N` — disabled
- `D` — value from the Trade Catalog module settings.

The response returns the resulting value `Y` or `N` ||
|| **canBuyZero**
[`string`](../../data-types.md) | Allow purchase when the product is out of stock.

Possible values:

- `Y` — yes
- `N` — no
- `D` — value from the Trade Catalog module settings.

The response returns the resulting value `Y` or `N` ||
|| **subscribe**
[`string`](../../data-types.md) | Allow subscription to the product.

Possible values:

- `Y` — yes
- `N` — no
- `D` — value from the Trade Catalog module settings.

The response returns the resulting value `Y` or `N` ||
|| **vatId**
[`catalog_vat.id`](../data-types.md#catalog_vat) | VAT rate identifier. You can retrieve the identifier using the [catalog.vat.list](../vat/catalog-vat-list.md) method.

The method does not check that the rate exists: a nonexistent identifier is saved without an error ||
|| **vatIncluded**
[`string`](../../data-types.md) | VAT is included in the price.

Possible values:

- `Y` — yes
- `N` — no ||
|| **purchasingPrice**
[`double`](../../data-types.md) | Purchasing price. When inventory management is enabled, the value is not retained: the method returns no error, but the field keeps its previous value ||
|| **purchasingCurrency**
[`string`](../../data-types.md) | Purchasing price currency, for example, `EUR`. You can retrieve the list of currencies using the [crm.currency.list](../../crm/currency/crm-currency-list.md) method.

When inventory management is enabled, the value is not retained: the method returns no error, but the field keeps its previous value ||
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
[`catalog_measure.id`](../data-types.md#catalog_measure) | Unit of measure identifier. You can retrieve the identifier using the [catalog.measure.list](../measure/catalog-measure-list.md) method.

The method does not check that the unit of measure exists: a nonexistent identifier is saved without an error ||
|| **recurSchemeLength**
[`integer`](../../data-types.md) | Payment period length. Used only in the [on-premise version](../../../settings/cloud-and-on-premise/index.md) for selling content ||
|| **recurSchemeType**
[`string`](../../data-types.md) | Time unit of the payment period.

Possible values:

- `H` — hour
- `D` — day
- `W` — week
- `M` — month
- `Q` — quarter
- `S` — half-year
- `Y` — year.

Used only in the [on-premise version](../../../settings/cloud-and-on-premise/index.md) for selling content ||
|| **trialPriceId**
[`integer`](../../data-types.md) | Identifier of the product for trial payment. Used only in the [on-premise version](../../../settings/cloud-and-on-premise/index.md) for selling content ||
|| **withoutOrder**
[`string`](../../data-types.md) | Renewal without placing an order.

Possible values:

- `Y` — yes
- `N` — no.

Used only in the [on-premise version](../../../settings/cloud-and-on-premise/index.md) for selling content ||
|| **propertyN**
[`object` | `array`](../../data-types.md) | Product property value, where `N` is the property identifier, for example, `property301`.

Value format:

- `{"valueId": 4083, "value": "value"}` — for a single property
- `[{"valueId": 4085, "value": "value 1"}, {"value": "value 2"}]` — for a multiple property

`valueId` is the identifier of the current property value. You can retrieve the identifiers using the [catalog.product.get](./catalog-product-get.md) and [catalog.product.list](./catalog-product-list.md) methods. If the property has no value yet, pass only `value`.

Pass a single property value as an object: a string instead of an object clears the property value.

For a multiple property, values without `valueId` in the request are deleted. The exception is "File" properties: values that are not passed in the request are retained, but their `valueId` values change.

For a property with the `BoolEnum` `userType`, pass `Y` or `N` in `value`.

Changing values of "File" properties is described in the [How to Update and Delete Files](../../files/how-to-update-files.md) article ||
|#

The `id`, `type`, `available`, `bundle`, and `timestampX` fields are filled in automatically. The method skips fields that are not in the table without an error.

{% note warning "Working with Product Prices" %}

To change the product price, use the [catalog.price.*](../price/index.md) methods.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7651,"fields":{"name":"Basic T-shirt, white","previewText":"Cotton T-shirt, white","detailPicture":{"fileData":["photo.png","iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=="]},"property301":{"valueId":4083,"value":"TS-002"},"property305":[{"valueId":4085,"value":"Cotton"},{"value":"Linen"}]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7651,"fields":{"name":"Basic T-shirt, white","previewText":"Cotton T-shirt, white","detailPicture":{"fileData":["photo.png","iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=="]},"property301":{"valueId":4083,"value":"TS-002"},"property305":[{"valueId":4085,"value":"Cotton"},{"value":"Linen"}]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ProductResult = {
      element: {
        id: number,
        iblockId: number,
        iblockSectionId: number | null,
        iblockSection: number[] | null,
        name: string,
        code: string | null,
        active: string,
        xmlId: string,
        dateCreate: ISODate,
        timestampX: ISODate,
        previewPicture: { id: string, url: string, urlMachine: string } | null,
        detailPicture: { id: string, url: string, urlMachine: string } | null,
        property301: { value: string, valueId: string } | null,
        property305: Array<{ value: string, valueId: string }> | null,
        [key: string]: unknown,
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<ProductResult>({
        method: 'catalog.product.update',
        params: {
          id: 7651,
          fields: {
            name: 'Basic T-shirt, white',
            previewText: 'Cotton T-shirt, white',
            detailPicture: {
              fileData: ['photo.png', 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=='],
            },
            property301: {
              valueId: 4083,
              value: 'TS-002',
            },
            property305: [
              {
                valueId: 4085,
                value: 'Cotton',
              },
              {
                value: 'Linen',
              },
            ],
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Updated product id:', result.element.id, 'name:', result.element.name)
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
      async function updateProduct() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.product.update',
            params: {
              id: 7651,
              fields: {
                name: 'Basic T-shirt, white',
                previewText: 'Cotton T-shirt, white',
                detailPicture: {
                  fileData: ['photo.png', 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=='],
                },
                property301: {
                  valueId: 4083,
                  value: 'TS-002',
                },
                property305: [
                  {
                    valueId: 4085,
                    value: 'Cotton',
                  },
                  {
                    value: 'Linen',
                  },
                ],
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
          console.info('Updated product id:', result.element.id, 'name:', result.element.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateProduct)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.update(
            id=7651,
            fields={
                "name": "Basic T-shirt, white",
                "previewText": "Cotton T-shirt, white",
                "detailPicture": {
                    "fileData": ["photo.png", "iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=="],
                },
                "property301": {
                    "valueId": 4083,
                    "value": "TS-002",
                },
                "property305": [
                    {
                        "valueId": 4085,
                        "value": "Cotton",
                    },
                    {
                        "value": "Linen",
                    },
                ],
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
                'catalog.product.update',
                [
                    'id'     => 7651,
                    'fields' => [
                        'name'          => 'Basic T-shirt, white',
                        'previewText'   => 'Cotton T-shirt, white',
                        'detailPicture' => [
                            'fileData' => ['photo.png', 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=='],
                        ],
                        'property301'   => [
                            'valueId' => 4083,
                            'value'   => 'TS-002',
                        ],
                        'property305'   => [
                            [
                                'valueId' => 4085,
                                'value'   => 'Cotton',
                            ],
                            [
                                'value' => 'Linen',
                            ],
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating product: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.product.update',
        {
            'id': 7651,
            'fields': {
                'name': 'Basic T-shirt, white',
                'previewText': 'Cotton T-shirt, white',
                'detailPicture': {
                    'fileData': ['photo.png', 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg==']
                },
                'property301': {
                    'valueId': 4083,
                    'value': 'TS-002'
                },
                'property305': [
                    {
                        'valueId': 4085,
                        'value': 'Cotton'
                    },
                    {
                        'value': 'Linen'
                    }
                ]
            }
        },
        function(result)
        {
            if(result.error())
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
        'catalog.product.update',
        [
            'id'     => 7651,
            'fields' => [
                'name'          => 'Basic T-shirt, white',
                'previewText'   => 'Cotton T-shirt, white',
                'detailPicture' => [
                    'fileData' => ['photo.png', 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=='],
                ],
                'property301'   => [
                    'valueId' => 4083,
                    'value'   => 'TS-002',
                ],
                'property305'   => [
                    [
                        'valueId' => 4085,
                        'value'   => 'Cotton',
                    ],
                    [
                        'value' => 'Linen',
                    ],
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
    res, err := client.Core().Call(ctx, "catalog.product.update", b24.Params{
    	"id":     7651,
    	"fields": b24.Params{
    		"name":          "Basic T-shirt, white",
    		"previewText":   "Cotton T-shirt, white",
    		"detailPicture": b24.Params{
    			"fileData": []string{"photo.png", "iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=="},
    		},
    		"property301":   b24.Params{
    			"valueId": 4083,
    			"value":   "TS-002",
    		},
    		"property305":   []b24.Params{
    			b24.Params{
    				"valueId": 4085,
    				"value":   "Cotton",
    			},
    			b24.Params{
    				"value": "Linen",
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.product.update: %w", err)
    }

    // The method wraps the response in an object with the "element" key.
    raw, ok := b24.Unwrap(res.Result, "element")
    if !ok {
    	return fmt.Errorf("no element key in the response")
    }

    var item struct {
    	ID              int    `json:"id"`
    	IblockID        int    `json:"iblockId"`
    	IblockSectionID int    `json:"iblockSectionId"`
    	Name            string `json:"name"`
    	Code            string `json:"code"`
    	Active          string `json:"active"`
    	XMLID           string `json:"xmlId"`
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
        "element": {
            "active": "Y",
            "available": "Y",
            "barcodeMulti": "N",
            "bundle": "N",
            "canBuyZero": "Y",
            "code": "tshirt-basic",
            "createdBy": 1269,
            "dateActiveFrom": null,
            "dateActiveTo": null,
            "dateCreate": "2026-11-05T18:23:16+01:00",
            "detailPicture": {
                "id": "40043",
                "url": "/rest/catalog.product.download?fields%5BfieldName%5D=detailPicture&fields%5BfileId%5D=40043&fields%5BproductId%5D=7651",
                "urlMachine": "/rest/catalog.product.download?fields%5BfieldName%5D=detailPicture&fields%5BfileId%5D=40043&fields%5BproductId%5D=7651"
            },
            "detailText": null,
            "detailTextType": "text",
            "height": null,
            "iblockId": 25,
            "iblockSection": [
                39,
                41
            ],
            "iblockSectionId": 41,
            "id": 7651,
            "length": null,
            "measure": 9,
            "modifiedBy": 1269,
            "name": "Basic T-shirt, white",
            "previewPicture": {
                "id": "40041",
                "url": "/rest/catalog.product.download?fields%5BfieldName%5D=previewPicture&fields%5BfileId%5D=40041&fields%5BproductId%5D=7651",
                "urlMachine": "/rest/catalog.product.download?fields%5BfieldName%5D=previewPicture&fields%5BfileId%5D=40041&fields%5BproductId%5D=7651"
            },
            "previewText": "Cotton T-shirt, white",
            "previewTextType": "text",
            "property301": {
                "value": "TS-002",
                "valueId": "4083"
            },
            "property305": [
                {
                    "value": "Cotton",
                    "valueId": "4085"
                },
                {
                    "value": "Linen",
                    "valueId": "4089"
                }
            ],
            "purchasingCurrency": null,
            "purchasingPrice": null,
            "quantity": null,
            "quantityReserved": null,
            "quantityTrace": "Y",
            "recurSchemeLength": null,
            "recurSchemeType": "D",
            "sort": 500,
            "subscribe": "Y",
            "timestampX": "2026-11-05T18:23:29+01:00",
            "trialPriceId": null,
            "type": 1,
            "vatId": 1,
            "vatIncluded": "Y",
            "weight": 200,
            "width": null,
            "withoutOrder": "N",
            "xmlId": "7651"
        }
    },
    "time": {
        "start": 1793892209,
        "finish": 1793892209.31875,
        "duration": 0.31874990463256836,
        "processing": 0,
        "date_start": "2026-11-05T18:23:29+01:00",
        "date_finish": "2026-11-05T18:23:29+01:00",
        "operating_reset_at": 1793892600,
        "operating": 0.7716009616851807
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Request result containing the updated product ||
|| **result.element**
[`catalog_product`](../data-types.md#catalog_product) | Object with information about the updated product ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"200040300010",
    "error_description":"Access Denied"
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300043` | `Access Denied` | No permission to edit the information block element ||
|| `400` | `200040300050` | `Access Denied` | The section in `iblockSectionId` was not found, or there is no permission to link the product to the section ||
|| `400` | `200040300040` | `Access Denied` | No permission to edit the product ||
|| `400` | `200040300010` | `Access Denied` | No permission to view the product catalog ||
|| `400` | `200040300000` | `Iblock Not Found` | The product information block was not found ||
|| `400` | `100` | `Could not find value for parameter {id}` | The required `id` parameter was not provided ||
|| `400` | `100` | `Could not find value for parameter {fields}` | The required `fields` parameter was not provided ||
|| `400` | Empty value | `product does not exist.` | The product with the specified `id` was not found ||
|| `400` | `0` | `User createdBy is not exists` or `User modifiedBy is not exists` | The user in `createdBy` or `modifiedBy` was not found ||
|| `400` | `0` | `Name is not specified.<br>` | An empty string was passed in `name` ||
|| `400` | `0` | `Incorrect starting date format.<br>` | A `dateActiveFrom` value was passed. For `dateActiveTo`, the error text is `Incorrect expiration date format.<br>` ||
|| `400` | `0` | `Internal error updating product. Try updating again.` | An internal error occurred while saving the product ||
|#

Validation of the changed fields and product properties can return additional errors that describe a specific field.

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-add.md)
- [{#T}](./catalog-product-get.md)
- [{#T}](./catalog-product-list.md)
- [{#T}](./catalog-product-download.md)
- [{#T}](./catalog-product-delete.md)
- [{#T}](./catalog-product-get-fields-by-filter.md)
