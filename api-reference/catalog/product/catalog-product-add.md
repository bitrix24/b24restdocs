# Add Product catalog.product.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Create product" permission and one of the following permissions: "View product catalog", "View catalog products", or "Edit selling price"

The `catalog.product.add` method adds a product to the trade catalog and returns its fields.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Object with the fields of the new product [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **iblockId***
[`catalog_catalog.id`](../data-types.md#catalog_catalog) | Identifier of the trade catalog information block. You can retrieve the identifier using the [catalog.catalog.list](../catalog/catalog-catalog-list.md) method.

Pass the products information block, not the variations one: with the variations information block, the method creates a variation without a parent product. Add variations using the [catalog.product.offer.add](./offer/catalog-product-offer-add.md) method ||
|| **name***
[`string`](../../data-types.md) | Product name ||
|| **active**
[`string`](../../data-types.md) | Product activity.

Possible values:

- `Y` — active
- `N` — inactive

Other values are saved as `N`

Default: `Y` ||
|| **code**
[`string`](../../data-types.md) | Symbolic code ||
|| **xmlId**
[`string`](../../data-types.md) | External code. If not passed, it matches the product identifier ||
|| **sort**
[`integer`](../../data-types.md) | Sorting

Default: `500` ||
|| **createdBy**
[`user.id`](../../data-types.md) | Identifier of the user who created the product. You can retrieve the identifier using the [user.get](../../user/user-get.md) method.

Default: the user who called the method ||
|| **modifiedBy**
[`user.id`](../../data-types.md) | Identifier of the user who modified the product.

Default: the user who called the method ||
|| **dateActiveFrom**
[`datetime`](../../data-types.md) | Activity start date. Currently the date cannot be set: a string in any format causes the "Incorrect starting date format" error, and a number is ignored. `null` is accepted ||
|| **dateActiveTo**
[`datetime`](../../data-types.md) | Activity end date. Currently the date cannot be set: a string in any format causes the "Incorrect expiration date format" error, and a number is ignored. `null` is accepted ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Creation date. The field is filled in automatically, so you do not need to pass it: currently a passed date is saved incorrectly, for example, `2024-05-27T10:00:00+01:00` is saved as `2001-11-30T00:00:00+01:00` ||
|| **iblockSectionId**
[`catalog_section.id`](../data-types.md#catalog_section) | Identifier of the main section. You can retrieve the identifier using the [catalog.section.list](../section/catalog-section-list.md) method.

For a nonexistent section, the method returns the `200040300050` error ||
|| **iblockSection**
[`catalog_section.id[]`](../data-types.md#catalog_section) | Array of identifiers of the sections to link the product to, for example, `[41, 39]`. Set the main section in `iblockSectionId` ||
|| **previewText**
[`string`](../../data-types.md) | Preview description ||
|| **previewTextType**
[`string`](../../data-types.md) | Type of the preview description.

Possible values:

- `text` — text
- `html` — HTML

Default: `text` ||
|| **previewPicture**
[`object`](../../data-types.md) | Preview picture. An object in the format `{"fileData": ["photo.png", "base64"]}`: the first element is the file name with the extension, the second is the file content in [Base64](../../files/how-to-upload-files.md) encoding.

To delete the picture, pass `{"remove": "Y"}` ||
|| **detailText**
[`string`](../../data-types.md) | Detailed description ||
|| **detailTextType**
[`string`](../../data-types.md) | Type of the detailed description.

Possible values:

- `text` — text
- `html` — HTML

Default: `text` ||
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
- `D` — value from the Trade Catalog module settings

Default: `D`.

The response returns the resulting value `Y` or `N` ||
|| **canBuyZero**
[`string`](../../data-types.md) | Allow purchase when the product is out of stock.

Possible values:

- `Y` — yes
- `N` — no
- `D` — value from the Trade Catalog module settings

Default: `D`.

The response returns the resulting value `Y` or `N` ||
|| **subscribe**
[`string`](../../data-types.md) | Allow subscription to the product.

Possible values:

- `Y` — yes
- `N` — no
- `D` — value from the Trade Catalog module settings

Default: `D`.

The response returns the resulting value `Y` or `N` ||
|| **vatId**
[`catalog_vat.id`](../data-types.md#catalog_vat) | VAT rate identifier. You can retrieve the identifier using the [catalog.vat.list](../vat/catalog-vat-list.md) method ||
|| **vatIncluded**
[`string`](../../data-types.md) | VAT is included in the price.

Possible values:

- `Y` — yes
- `N` — no

Default: `N` ||
|| **purchasingPrice**
[`double`](../../data-types.md) | Purchasing price. When inventory management is enabled, the value is not retained: the method returns no error, but the field keeps its previous value ||
|| **purchasingCurrency**
[`string`](../../data-types.md) | Purchasing price currency, for example, `EUR`. You can retrieve the list of currencies using the [crm.currency.list](../../crm/currency/crm-currency-list.md) method.

When inventory management is enabled, the value is not retained: the method returns no error, but the field keeps its previous value ||
|| **barcodeMulti**
[`string`](../../data-types.md) | Unique barcode for each item.

Possible values:

- `Y` — yes
- `N` — no

Default: `N` ||
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
- `Y` — year

Default: `D`.

Used only in the [on-premise version](../../../settings/cloud-and-on-premise/index.md) for selling content ||
|| **trialPriceId**
[`integer`](../../data-types.md) | Identifier of the product for trial payment. Used only in the [on-premise version](../../../settings/cloud-and-on-premise/index.md) for selling content ||
|| **withoutOrder**
[`string`](../../data-types.md) | Renewal without placing an order.

Possible values:

- `Y` — yes
- `N` — no

Default: `N`.

Used only in the [on-premise version](../../../settings/cloud-and-on-premise/index.md) for selling content ||
|| **propertyN**
[`any`](../../data-types.md) | Product property value, where `N` is the property identifier, for example, `property301`. You can retrieve property identifiers using the [catalog.productProperty.list](../product-property/catalog-product-property-list.md) method.

Pass a single property value as a string, a number, or a `{"value": "value"}` object. Pass a multiple property value as an array of values or `{"value": "value"}` objects.

The method skips properties with a nonexistent identifier without an error. Uploading values of "File" properties is described in the [How to Upload Files](../../files/how-to-upload-files.md) article ||
|#

The `id`, `type`, `available`, `bundle`, and `timestampX` fields are filled in automatically. The method skips fields that are not in the table without an error.

{% note warning "Working with Product Prices" %}

To set the product price, use the [catalog.price.*](../price/index.md) methods.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":25,"name":"Basic T-shirt","code":"tshirt-basic","iblockSectionId":41,"iblockSection":[41,39],"vatId":1,"vatIncluded":"Y","measure":9,"weight":200,"previewText":"Cotton T-shirt","previewPicture":{"fileData":["photo.png","iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=="]},"property301":"TS-001","property305":["Cotton","Polyester"]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":25,"name":"Basic T-shirt","code":"tshirt-basic","iblockSectionId":41,"iblockSection":[41,39],"vatId":1,"vatIncluded":"Y","measure":9,"weight":200,"previewText":"Cotton T-shirt","previewPicture":{"fileData":["photo.png","iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=="]},"property301":"TS-001","property305":["Cotton","Polyester"]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.add
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
        method: 'catalog.product.add',
        params: {
          fields: {
            iblockId: 25,
            name: 'Basic T-shirt',
            code: 'tshirt-basic',
            iblockSectionId: 41,
            iblockSection: [41, 39],
            vatId: 1,
            vatIncluded: 'Y',
            measure: 9,
            weight: 200,
            previewText: 'Cotton T-shirt',
            previewPicture: {
              fileData: ['photo.png', 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=='],
            },
            property301: 'TS-001',
            property305: ['Cotton', 'Polyester'],
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Added product id:', result.element.id, 'name:', result.element.name)
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
      async function addProduct() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.product.add',
            params: {
              fields: {
                iblockId: 25,
                name: 'Basic T-shirt',
                code: 'tshirt-basic',
                iblockSectionId: 41,
                iblockSection: [41, 39],
                vatId: 1,
                vatIncluded: 'Y',
                measure: 9,
                weight: 200,
                previewText: 'Cotton T-shirt',
                previewPicture: {
                  fileData: ['photo.png', 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=='],
                },
                property301: 'TS-001',
                property305: ['Cotton', 'Polyester'],
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
          console.info('Added product id:', result.element.id, 'name:', result.element.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addProduct)
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
                "iblockSection": [41, 39],
                "vatId": 1,
                "vatIncluded": "Y",
                "measure": 9,
                "weight": 200,
                "previewText": "Cotton T-shirt",
                "previewPicture": {
                    "fileData": ["photo.png", "iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=="],
                },
                "property301": "TS-001",
                "property305": ["Cotton", "Polyester"],
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
                'catalog.product.add',
                [
                    'fields' => [
                        'iblockId'        => 25,
                        'name'            => 'Basic T-shirt',
                        'code'            => 'tshirt-basic',
                        'iblockSectionId' => 41,
                        'iblockSection'   => [41, 39],
                        'vatId'           => 1,
                        'vatIncluded'     => 'Y',
                        'measure'         => 9,
                        'weight'          => 200,
                        'previewText'     => 'Cotton T-shirt',
                        'previewPicture'  => [
                            'fileData' => ['photo.png', 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=='],
                        ],
                        'property301'     => 'TS-001',
                        'property305'     => ['Cotton', 'Polyester'],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding product: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.product.add',
        {
            'fields': {
                'iblockId': 25,
                'name': 'Basic T-shirt',
                'code': 'tshirt-basic',
                'iblockSectionId': 41,
                'iblockSection': [41, 39],
                'vatId': 1,
                'vatIncluded': 'Y',
                'measure': 9,
                'weight': 200,
                'previewText': 'Cotton T-shirt',
                'previewPicture': {
                    'fileData': ['photo.png', 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg==']
                },
                'property301': 'TS-001',
                'property305': ['Cotton', 'Polyester']
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
        'catalog.product.add',
        [
            'fields' => [
                'iblockId'        => 25,
                'name'            => 'Basic T-shirt',
                'code'            => 'tshirt-basic',
                'iblockSectionId' => 41,
                'iblockSection'   => [41, 39],
                'vatId'           => 1,
                'vatIncluded'     => 'Y',
                'measure'         => 9,
                'weight'          => 200,
                'previewText'     => 'Cotton T-shirt',
                'previewPicture'  => [
                    'fileData' => ['photo.png', 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=='],
                ],
                'property301'     => 'TS-001',
                'property305'     => ['Cotton', 'Polyester'],
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
    	"fields": b24.Params{
    		"iblockId":        25,
    		"name":            "Basic T-shirt",
    		"code":            "tshirt-basic",
    		"iblockSectionId": 41,
    		"iblockSection":   []int{41, 39},
    		"vatId":           1,
    		"vatIncluded":     "Y",
    		"measure":         9,
    		"weight":          200,
    		"previewText":     "Cotton T-shirt",
    		"previewPicture":  b24.Params{
    			"fileData": []string{"photo.png", "iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=="},
    		},
    		"property301":     "TS-001",
    		"property305":     []string{"Cotton", "Polyester"},
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.product.add: %w", err)
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
            "detailPicture": null,
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
            "name": "Basic T-shirt",
            "previewPicture": {
                "id": "40041",
                "url": "/rest/catalog.product.download?fields%5BfieldName%5D=previewPicture&fields%5BfileId%5D=40041&fields%5BproductId%5D=7651",
                "urlMachine": "/rest/catalog.product.download?fields%5BfieldName%5D=previewPicture&fields%5BfileId%5D=40041&fields%5BproductId%5D=7651"
            },
            "previewText": "Cotton T-shirt",
            "previewTextType": "text",
            "property301": {
                "value": "TS-001",
                "valueId": "4083"
            },
            "property305": [
                {
                    "value": "Cotton",
                    "valueId": "4085"
                },
                {
                    "value": "Polyester",
                    "valueId": "4087"
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
            "timestampX": "2026-11-05T18:23:16+01:00",
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
        "start": 1793892196,
        "finish": 1793892196.679604,
        "duration": 0.6796040534973145,
        "processing": 0,
        "date_start": "2026-11-05T18:23:16+01:00",
        "date_finish": "2026-11-05T18:23:16+01:00",
        "operating_reset_at": 1793892360,
        "operating": 1.0158729553222656
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Request result containing the created product ||
|| **result.element**
[`catalog_product`](../data-types.md#catalog_product) | Object with information about the added product ||
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
|| `400` | `200040300050` | `Access Denied` | The section in `iblockSectionId` was not found, or there is no permission to link the product to the section ||
|| `400` | `200040300040` | `Access Denied` | No permission to create a product ||
|| `400` | `200040300043` | `Access Denied` | No permission to add an information block element ||
|| `400` | `200040300010` | `Access Denied` | No permission to view the product catalog ||
|| `400` | `200040300000` | `Iblock Not Found` | The information block in `fields.iblockId` was not found ||
|| `400` | `100` | `Could not find value for parameter {fields}` | The required `fields` parameter was not provided ||
|| `400` | `0` | `Required fields: name, iblockId` | Required fields are missing in `fields`. The message lists only the missing fields ||
|| `400` | `0` | `Incorrect starting date format.<br>` | A `dateActiveFrom` value was passed. For `dateActiveTo`, the error text is `Incorrect expiration date format.<br>` ||
|| `400` | `0` | `VAT vatId is not exists` | The VAT rate in `fields.vatId` was not found ||
|| `400` | `0` | `Currency purchasingCurrency is not exists` | The purchasing price currency was not found ||
|| `400` | `0` | `User createdBy is not exists` or `User modifiedBy is not exists` | The user in `createdBy` or `modifiedBy` was not found ||
|| `400` | `0` | `Internal error adding product. Try adding again.` | An internal error occurred while saving the product ||
|#

Validation of required fields and product properties can return additional errors that describe a specific field.

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-update.md)
- [{#T}](./catalog-product-get.md)
- [{#T}](./catalog-product-list.md)
- [{#T}](../../../tutorials/catalog/index.md)
- [{#T}](./catalog-product-download.md)
- [{#T}](./catalog-product-delete.md)
- [{#T}](./catalog-product-get-fields-by-filter.md)
