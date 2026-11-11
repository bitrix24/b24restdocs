# Add Parent Product catalog.product.sku.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Create product" access permission, a catalog view permission ("View product catalog", "View catalog products", or "Edit selling price"), and the permission to add elements to the catalog information block

The `catalog.product.sku.add` method adds a parent product to the trade catalog. A new parent product gets type `6`, a parent product without variations. After the first variation is added using the [catalog.product.offer.add](../offer/catalog-product-offer-add.md) method, the type changes to `3`.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Field values for adding a parent product ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **iblockId***
[`catalog_catalog.id`](../../data-types.md#catalog_catalog) | Identifier of the product trade catalog information block.

You can retrieve the identifier using the [catalog.catalog.list](../../catalog/catalog-catalog-list.md) method: it is the catalog with `productIblockId` equal to `null`. The variation information block is not suitable ||
|| **name***
[`string`](../../../data-types.md) | Name of the parent product ||
|| **active**
[`string`](../../../data-types.md) | Whether the parent product is active. Possible values:
- `Y` — active
- `N` — inactive

The method saves any other value as `N`.

Default is `Y` ||
|| **code**
[`string`](../../../data-types.md) | Symbolic code. The method does not check that the code is unique ||
|| **xmlId**
[`string`](../../../data-types.md) | External code. Use it to link the product to a record in an external system.

Equals the product identifier by default ||
|| **sort**
[`integer`](../../../data-types.md) | Sort index.

The method saves a value that is not a number as `null` without an error.

Default is `500` ||
|| **iblockSectionId**
[`catalog_section.id`](../../data-types.md#catalog_section) | Identifier of the main catalog section.

You can retrieve section identifiers using the [catalog.section.list](../../section/catalog-section-list.md) method ||
|| **iblockSection**
[`catalog_section.id[]`](../../data-types.md#catalog_section) | Array of identifiers of all sections to link the parent product to ||
|| **previewText**
[`string`](../../../data-types.md) | Preview description ||
|| **previewTextType**
[`string`](../../../data-types.md) | Type of the preview description. Possible values:
- `text` — text
- `html` — HTML

The method saves any other value as `text`.

Default is `text` ||
|| **detailText**
[`string`](../../../data-types.md) | Detailed description ||
|| **detailTextType**
[`string`](../../../data-types.md) | Type of the detailed description. Possible values:
- `text` — text
- `html` — HTML

The method saves any other value as `text`.

Default is `text` ||
|| **previewPicture**
[`object`](../../../data-types.md) | Preview image. An object in the format `{"fileData": ["file_name.png", "base64_content"]}` ||
|| **detailPicture**
[`object`](../../../data-types.md) | Detailed image. An object in the format `{"fileData": ["file_name.png", "base64_content"]}` ||
|| **createdBy**
[`user.id`](../../../data-types.md) | Identifier of the user who created the product.

Default is the current user ||
|| **modifiedBy**
[`user.id`](../../../data-types.md) | Identifier of the user who modified the product.

Default is the current user ||
|| **propertyN**
[`any`](../../../data-types.md) | Product property value, where `N` is the property identifier, for example `property303`.

You can pass the value as a string, a number, or a `{"value": ...}` object. For a multiple property, pass an array of values.

The [catalog.product.sku.getFieldsByFilter](./catalog-product-sku-get-fields-by-filter.md) method returns the list of information block properties ||
|#

The method fills in the `id`, `type`, `available`, `bundle`, `dateCreate`, and `timestampX` fields itself. The method skips unknown fields in `fields` without an error.

Currently, the `dateActiveFrom` and `dateActiveTo` activity dates cannot be set: the method rejects any date format with the `Incorrect starting date format.<br>` error.

The method skips the trade catalog fields of a parent product, `canBuyZero`, `vatId`, `vatIncluded`, `quantity`, `purchasingPrice`, `purchasingCurrency`, `measure`, `weight`, `length`, `width`, and `height`, without an error and does not retain them in cloud Bitrix24. They can be changed only in the on-premise version with the "Enable the Commercial Catalog tab for products with SKU's" option enabled. Set the values for variations using the [catalog.product.offer.add](../offer/catalog-product-offer-add.md) method.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":25,"name":"Basic T-shirt","code":"basic-t-shirt","xmlId":"tshirt-basic-01","sort":200,"iblockSectionId":31,"previewText":"Loose-fit cotton T-shirt","detailText":"<p>T-shirt made of 100% cotton</p>","detailTextType":"html","property303":"Textile Plus","property305":["Cotton","Elastane"]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.sku.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":25,"name":"Basic T-shirt","code":"basic-t-shirt","xmlId":"tshirt-basic-01","sort":200,"iblockSectionId":31,"previewText":"Loose-fit cotton T-shirt","detailText":"<p>T-shirt made of 100% cotton</p>","detailTextType":"html","property303":"Textile Plus","property305":["Cotton","Elastane"]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.sku.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type PropertyValue = { value: string; valueId: string }
    type Picture = { id: string; url: string; urlMachine: string }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SkuAddResult = {
      sku: {
        active: string
        available: string
        bundle: string
        code: string | null
        createdBy: number
        dateActiveFrom: ISODate | null
        dateActiveTo: ISODate | null
        dateCreate: ISODate
        detailPicture: Picture | null
        detailText: string | null
        detailTextType: string
        iblockId: number
        iblockSection: number[] | null
        iblockSectionId: number | null
        id: number
        modifiedBy: number
        name: string
        previewPicture: Picture | null
        previewText: string | null
        previewTextType: string
        property303: PropertyValue | null
        property305: PropertyValue[] | null
        sort: number | null
        timestampX: ISODate
        type: number
        xmlId: string | null
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<SkuAddResult>({
        method: 'catalog.product.sku.add',
        params: {
          fields: {
            iblockId: 25,
            name: 'Basic T-shirt',
            code: 'basic-t-shirt',
            xmlId: 'tshirt-basic-01',
            sort: 200,
            iblockSectionId: 31,
            previewText: 'Loose-fit cotton T-shirt',
            detailText: '<p>T-shirt made of 100% cotton</p>',
            detailTextType: 'html',
            property303: 'Textile Plus',
            property305: ['Cotton', 'Elastane'],
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Added SKU product:', result.sku.id, result.sku.name)
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
      async function addSkuProduct() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.product.sku.add',
            params: {
              fields: {
                iblockId: 25,
                name: 'Basic T-shirt',
                code: 'basic-t-shirt',
                xmlId: 'tshirt-basic-01',
                sort: 200,
                iblockSectionId: 31,
                previewText: 'Loose-fit cotton T-shirt',
                detailText: '<p>T-shirt made of 100% cotton</p>',
                detailTextType: 'html',
                property303: 'Textile Plus',
                property305: ['Cotton', 'Elastane'],
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
          console.info('Added SKU product:', result.sku.id, result.sku.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addSkuProduct)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.sku.add(
            fields={
                "iblockId": 25,
                "name": "Basic T-shirt",
                "code": "basic-t-shirt",
                "xmlId": "tshirt-basic-01",
                "sort": 200,
                "iblockSectionId": 31,
                "previewText": "Loose-fit cotton T-shirt",
                "detailText": "<p>T-shirt made of 100% cotton</p>",
                "detailTextType": "html",
                "property303": "Textile Plus",
                "property305": ["Cotton", "Elastane"],
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
                'catalog.product.sku.add',
                [
                    'fields' => [
                        'iblockId'        => 25,
                        'name'            => 'Basic T-shirt',
                        'code'            => 'basic-t-shirt',
                        'xmlId'           => 'tshirt-basic-01',
                        'sort'            => 200,
                        'iblockSectionId' => 31,
                        'previewText'     => 'Loose-fit cotton T-shirt',
                        'detailText'      => '<p>T-shirt made of 100% cotton</p>',
                        'detailTextType'  => 'html',
                        'property303'     => 'Textile Plus',
                        'property305'     => ['Cotton', 'Elastane'],
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
        'catalog.product.sku.add',
        {
            fields: {
                iblockId: 25,
                name: 'Basic T-shirt',
                code: 'basic-t-shirt',
                xmlId: 'tshirt-basic-01',
                sort: 200,
                iblockSectionId: 31,
                previewText: 'Loose-fit cotton T-shirt',
                detailText: '<p>T-shirt made of 100% cotton</p>',
                detailTextType: 'html',
                property303: 'Textile Plus',
                property305: ['Cotton', 'Elastane'],
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
        'catalog.product.sku.add',
        [
            'fields' => [
                'iblockId' => 25,
                'name' => 'Basic T-shirt',
                'code' => 'basic-t-shirt',
                'xmlId' => 'tshirt-basic-01',
                'sort' => 200,
                'iblockSectionId' => 31,
                'previewText' => 'Loose-fit cotton T-shirt',
                'detailText' => '<p>T-shirt made of 100% cotton</p>',
                'detailTextType' => 'html',
                'property303' => 'Textile Plus',
                'property305' => ['Cotton', 'Elastane'],
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.product.sku.add", b24.Params{
    	"fields": b24.Params{
    		"iblockId":        25,
    		"name":            "Basic T-shirt",
    		"code":            "basic-t-shirt",
    		"xmlId":           "tshirt-basic-01",
    		"sort":            200,
    		"iblockSectionId": 31,
    		"previewText":     "Loose-fit cotton T-shirt",
    		"detailText":      "<p>T-shirt made of 100% cotton</p>",
    		"detailTextType":  "html",
    		"property303":     "Textile Plus",
    		"property305":     []string{"Cotton", "Elastane"},
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.product.sku.add: %w", err)
    }

    // The method wraps the response in an object with the "sku" key.
    raw, ok := b24.Unwrap(res.Result, "sku")
    if !ok {
    	return fmt.Errorf("no sku key in the response")
    }

    // Optional fields can be null, so pointers are used for them.
    type propertyValue struct {
    	Value   string `json:"value"`
    	ValueID string `json:"valueId"`
    }
    var item struct {
    	ID              b24.ID          `json:"id"`
    	IblockID        int             `json:"iblockId"`
    	Name            string          `json:"name"`
    	Code            *string         `json:"code"`
    	XMLID           *string         `json:"xmlId"`
    	Sort            *int            `json:"sort"`
    	Type            int             `json:"type"`
    	IblockSectionID *int            `json:"iblockSectionId"`
    	Property303     *propertyValue  `json:"property303"`
    	Property305     []propertyValue `json:"property305"`
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
        "sku": {
            "active": "Y",
            "available": "N",
            "bundle": "N",
            "code": "basic-t-shirt",
            "createdBy": 1269,
            "dateActiveFrom": null,
            "dateActiveTo": null,
            "dateCreate": "2026-11-05T18:38:10+01:00",
            "detailPicture": null,
            "detailText": "<p>T-shirt made of 100% cotton</p>",
            "detailTextType": "html",
            "iblockId": 25,
            "iblockSection": [
                31
            ],
            "iblockSectionId": 31,
            "id": 7679,
            "modifiedBy": 1269,
            "name": "Basic T-shirt",
            "previewPicture": null,
            "previewText": "Loose-fit cotton T-shirt",
            "previewTextType": "text",
            "property303": {
                "value": "Textile Plus",
                "valueId": "4111"
            },
            "property305": [
                {
                    "value": "Cotton",
                    "valueId": "4113"
                },
                {
                    "value": "Elastane",
                    "valueId": "4115"
                }
            ],
            "sort": 200,
            "timestampX": "2026-11-05T18:38:10+01:00",
            "type": 6,
            "xmlId": "tshirt-basic-01"
        }
    },
    "time": {
        "start": 1793893090,
        "finish": 1793893090.500774,
        "duration": 0.5007739067077637,
        "processing": 0,
        "date_start": "2026-11-05T18:38:10+01:00",
        "date_finish": "2026-11-05T18:38:10+01:00",
        "operating_reset_at": 1793893140,
        "operating": 0.37030506134033203
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **result.sku**
[`catalog_product_sku`](../../data-types.md#catalog_product_sku) | Object with the fields of the created parent product.

The response contains all information block properties as `propertyN` keys. Empty properties have the `null` value, filled properties are `{"value": ..., "valueId": ...}` objects, and multiple properties are arrays of such objects. The example shows only the `property303` and `property305` properties.

The `previewPicture` and `detailPicture` images are returned as an object with the `id` file identifier and the `url` and `urlMachine` download links. You can download the file using the [catalog.product.sku.download](./catalog-product-sku-download.md) method ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Required fields: name"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user does not have a permission to view the catalog ||
|| `400` | `200040300040` | Access Denied | The user does not have the "Create product" permission ||
|| `400` | `200040300043` | Access Denied | The user does not have the permission to add elements to the catalog information block ||
|| `400` | `200040300050` | Access Denied | The section from `iblockSectionId` does not exist, or the user does not have the permission to link products to it ||
|| `400` | `100` | Could not find value for parameter {fields} | The `fields` parameter is not provided ||
|| `400` | `0` | Incorrect fields format | `fields` is not an object ||
|| `400` | `0` | iblock is not catalog | `iblockId` is not provided, or the information block is not a trade catalog ||
|| `400` | `0` | productType is not allowed for this catalog | `iblockId` contains the variation information block ||
|| `400` | `0` | Required fields: name | The `name` field is not provided ||
|| `400` | `0` | Wrong format of field `name`. | The field contains an array or an object instead of a value. The text contains the field name ||
|| `400` | `0` | User createdBy is not exists | The user from `createdBy` is not found. For `modifiedBy`, the error is `User modifiedBy is not exists` ||
|| `400` | `0` | Incorrect starting date format.<br> | The `dateActiveFrom` field is passed. For `dateActiveTo`, the error is `Incorrect expiration date format.<br>` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-sku-update.md)
- [{#T}](./catalog-product-sku-get.md)
- [{#T}](./catalog-product-sku-list.md)
- [{#T}](./catalog-product-sku-download.md)
- [{#T}](./catalog-product-sku-delete.md)
- [{#T}](./catalog-product-sku-get-fields-by-filter.md)
