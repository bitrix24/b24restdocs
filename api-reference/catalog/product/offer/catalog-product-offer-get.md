# Get Product Variation Field Values catalog.product.offer.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to read the variation in the information block and one of the permissions: "View product catalog", "View catalog products", or "Edit selling price"

The method `catalog.product.offer.get` returns the values of all fields and properties of a product variation by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_product_offer.id`](../../data-types.md#catalog_product_offer) | Identifier of the variation.

You can retrieve variation identifiers using the [catalog.product.offer.list](./catalog-product-offer-list.md) method ||
|#

If you pass the identifier of a product rather than a variation in `id`, the method returns an empty array `{"offer": []}` without an error.

The method does not return variation prices. To retrieve prices, use the [catalog.price.*](../../price/index.md) methods.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7695}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.offer.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7695,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.offer.get
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
        method: 'catalog.product.offer.get',
        params: {
          id: 7695,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Offer:', result.offer.id, result.offer.name, result.offer.parentId?.value)
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
      async function getOffer() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.product.offer.get',
            params: {
                  id: 7695,
                },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Offer:', result.offer.id, result.offer.name, result.offer.parentId?.value)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getOffer)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.offer.get(
            bitrix_id=7695,
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
                'catalog.product.offer.get',
                [
                    'id' => 7695,
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
        'catalog.product.offer.get',
        {
            id: 7695,
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
        'catalog.product.offer.get',
        [
            'id' => 7695,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.product.offer.get", b24.Params{
    	"id": 7695,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.offer.get: %w", err)
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
        "start": 1793895573,
        "finish": 1793895573.636081,
        "duration": 0.6360809803009033,
        "processing": 0,
        "date_start": "2026-11-05T19:19:33+01:00",
        "date_finish": "2026-11-05T19:19:33+01:00",
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
[`catalog_product_offer`](../../data-types.md#catalog_product_offer) | Object with all fields and properties of the variation. Empty fields are returned as `null`.

The parent product is returned in the `parentId` field as an object `{"value": ..., "valueId": ...}`, where `value` is the parent product identifier. A property is returned as the same object, for a list property with the option text in `valueEnum`, and for a multiple property as an array of objects.

A picture is returned as an object with the file identifier `id` and relative links `url` and `urlMachine` to the [catalog.product.download](../catalog-product-download.md) method. You can also download the file using the [catalog.product.offer.download](./catalog-product-offer-download.md) method.

The `purchasingPrice` and `purchasingCurrency` fields are returned only if the user has the "View purchase price" permission. The purchasing price is returned as a string ||
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
|| `400` | `200040300040` | Access Denied | The user does not have permission to read this variation in the information block ||
|| `400` | Empty value | offer does not exist. | No variation exists with the specified `id`. The method returns the same error if a non-numeric value is passed in `id` ||
|| `400` | `100` | Could not find value for parameter {id} | The `id` parameter was not provided ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-offer-add.md)
- [{#T}](./catalog-product-offer-update.md)
- [{#T}](./catalog-product-offer-list.md)
- [{#T}](./catalog-product-offer-download.md)
- [{#T}](./catalog-product-offer-delete.md)
- [{#T}](./catalog-product-offer-get-fields-by-filter.md)
