# Get Parent Product Field Values catalog.product.sku.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog", "View catalog products", or "Edit selling price" access permission and the permission to read the catalog information block element

The `catalog.product.sku.get` method returns the values of parent product fields by identifier.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_product_sku.id`](../../data-types.md#catalog_product_sku) | Identifier of the parent product.

You can retrieve parent product identifiers using the [catalog.product.sku.list](./catalog-product-sku-list.md) method ||
|#

## Code Examples

{% include [Note about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7679}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.sku.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7679,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.sku.get
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

    type SkuItem = {
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

    // Shape of the payload returned in result (match the "response handling" section of the page).
    // For a simple product or a variation the method returns an empty array.
    type SkuGetResult = {
      sku: SkuItem | []
    }

    try {
      const response = await $b24.actions.v2.call.make<SkuGetResult>({
        method: 'catalog.product.sku.get',
        params: {
          id: 7679,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const sku = response.getData()!.result.sku
        if (Array.isArray(sku)) {
          console.warn('The ID does not belong to a parent product')
        } else {
          console.info('SKU product:', sku.id, sku.name)
        }
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
      async function getSkuProduct() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.product.sku.get',
            params: {
              id: 7679,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const sku = response.getData().result.sku
          if (Array.isArray(sku)) {
            console.warn('The ID does not belong to a parent product')
            return
          }
          console.info('SKU product:', sku.id, sku.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getSkuProduct)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.sku.get(
            bitrix_id=7679,
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
                'catalog.product.sku.get',
                [
                    'id' => 7679,
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
        'catalog.product.sku.get',
        {
            id: 7679,
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
        'catalog.product.sku.get',
        [
            'id' => 7679,
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
    	"id": 7679,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.sku.get: %w", err)
    }

    // The method wraps the response in an object with the "sku" key.
    // For a simple product or a variation, "sku" contains an empty array.
    raw, ok := b24.Unwrap(res.Result, "sku")
    if !ok {
    	return fmt.Errorf("no sku key in the response")
    }
    if string(raw) == "[]" {
    	return fmt.Errorf("the product is not a parent product")
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
    	Active          string          `json:"active"`
    	Code            *string         `json:"code"`
    	Sort            *int            `json:"sort"`
    	Type            int             `json:"type"`
    	IblockSectionID *int            `json:"iblockSectionId"`
    	PreviewText     *string         `json:"previewText"`
    	Property303     *propertyValue  `json:"property303"`
    	Property305     []propertyValue `json:"property305"`
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
                "value": "Cotton Yard",
                "valueId": "4117"
            },
            "property305": [
                {
                    "value": "Cotton",
                    "valueId": "4119"
                }
            ],
            "sort": 300,
            "timestampX": "2026-11-05T18:38:23+01:00",
            "type": 6,
            "xmlId": "tshirt-basic-01"
        }
    },
    "time": {
        "start": 1793893104,
        "finish": 1793893104.8058,
        "duration": 0.8057999610900879,
        "processing": 0,
        "date_start": "2026-11-05T18:38:24+01:00",
        "date_finish": "2026-11-05T18:38:24+01:00",
        "operating_reset_at": 1793893680,
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
|| **result.sku**
[`catalog_product_sku`](../../data-types.md#catalog_product_sku) | Object with the parent product fields. Empty fields have the `null` value.

The response contains all information block properties as `propertyN` keys: a filled property is a `{"value": ..., "valueId": ...}` object, a multiple property is an array of such objects. The example shows only the `property303` and `property305` properties.

The method does not return trade catalog fields such as `canBuyZero` and `vatId`. You can retrieve them using the [catalog.product.sku.list](./catalog-product-sku-list.md) method by listing the fields in `select`.

If `id` belongs to a simple product or a variation, `sku` contains an empty array `[]` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "sku does not exist."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user does not have a permission to view the catalog ||
|| `400` | `200040300040` | Access Denied | The user does not have the permission to read the catalog information block element ||
|| `400` | Empty value | sku does not exist. | No product exists with the specified `id`. The method returns the same error if `id` is `0` or a string that is not a number ||
|| `400` | `100` | Could not find value for parameter {id} | The `id` parameter is not provided ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-sku-add.md)
- [{#T}](./catalog-product-sku-update.md)
- [{#T}](./catalog-product-sku-list.md)
- [{#T}](./catalog-product-sku-download.md)
- [{#T}](./catalog-product-sku-delete.md)
- [{#T}](./catalog-product-sku-get-fields-by-filter.md)
