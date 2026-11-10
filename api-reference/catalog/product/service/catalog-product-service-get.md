# Get Service Field Values catalog.product.service.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with one of the "View product catalog", "View catalog products", or "Edit selling price" access permissions and the permission to read elements of the catalog information block

The `catalog.product.service.get` method returns the field values of a service by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_product_service.id`](../../data-types.md#catalog_product_service) | Identifier of the service. You can retrieve the identifier using the [catalog.product.service.list](./catalog-product-service-list.md) method ||
|#

{% note warning "Working with Service Prices" %}

To get service prices, use the methods [catalog.price.*](../../price/index.md).

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7623}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.service.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7623,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.service.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type GetServiceResult = {
      service: {
        id: number
        name: string
        active: string
        available: string
        bundle: string
        code: string
        createdBy: number
        dateActiveFrom: ISODate | null
        dateActiveTo: ISODate | null
        dateCreate: ISODate | null
        detailPicture: { id: string; url: string; urlMachine: string } | null
        detailText: string | null
        detailTextType: string
        iblockId: number
        iblockSectionId: number | null
        modifiedBy: number
        previewPicture: { id: string; url: string; urlMachine: string } | null
        previewText: string | null
        previewTextType: string
        sort: number
        timestampX: ISODate | null
        type: number
        vatId: number | null
        vatIncluded: string
        xmlId: string | null
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<GetServiceResult>({
        method: 'catalog.product.service.get',
        params: {
          id: 7623,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Service:', result.service.id, result.service.name, result.service.active)
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
      async function getService() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.product.service.get',
            params: {
              id: 7623,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Service:', result.service.id, result.service.name, result.service.active)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getService)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.service.get(
            bitrix_id=7623,
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
                'catalog.product.service.get',
                [
                    'id' => 7623
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting product service: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.product.service.get', {
            id: 7623
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.product.service.get',
        [
            'id' => 7623
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.product.service.get", b24.Params{
    	"id": 7623,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.service.get: %w", err)
    }

    // The method wraps the response in an object with the "service" key.
    raw, ok := b24.Unwrap(res.Result, "service")
    if !ok {
    	return fmt.Errorf("no service key in the response")
    }

    var item struct {
    	Active         string `json:"active"`
    	Available      string `json:"available"`
    	Bundle         string `json:"bundle"`
    	Code           string `json:"code"`
    	CreatedBy      int    `json:"createdBy"`
    	DateActiveFrom string `json:"dateActiveFrom"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Active, item.Available)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "service": {
            "active": "Y",
            "available": "N",
            "bundle": "N",
            "code": "ac_installation",
            "createdBy": 1269,
            "dateActiveFrom": null,
            "dateActiveTo": null,
            "dateCreate": "2026-11-05T18:19:49+01:00",
            "detailPicture": null,
            "detailText": "<p>Installation of the indoor and outdoor units</p>",
            "detailTextType": "html",
            "iblockId": 25,
            "iblockSection": null,
            "iblockSectionId": null,
            "id": 7623,
            "measure": 9,
            "modifiedBy": 1269,
            "name": "Air conditioner installation",
            "previewPicture": {
                "id": "40055",
                "url": "/rest/catalog.product.download?fields%5BfieldName%5D=previewPicture&fields%5BfileId%5D=40055&fields%5BproductId%5D=7623",
                "urlMachine": "/rest/catalog.product.download?fields%5BfieldName%5D=previewPicture&fields%5BfileId%5D=40055&fields%5BproductId%5D=7623"
            },
            "previewText": "Turnkey installation of a split system up to 3.5 kW",
            "previewTextType": "text",
            "property301": {
                "value": "SRV-AC-01",
                "valueId": "4147"
            },
            "property303": {
                "value": "Klima Service GmbH",
                "valueId": "4061"
            },
            "property305": null,
            "sort": 200,
            "timestampX": "2026-11-05T19:09:53+01:00",
            "type": 7,
            "vatId": 9,
            "vatIncluded": "Y",
            "xmlId": "srv-ac-01"
        }
    },
    "time": {
        "start": 1793964025,
        "finish": 1793964025.810799,
        "duration": 0.8107988834381104,
        "processing": 0,
        "date_start": "2026-11-06T14:20:25+01:00",
        "date_finish": "2026-11-06T14:20:25+01:00",
        "operating_reset_at": 1793964600,
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
|| **result.service**
[`catalog_product_service`](../../data-types.md#catalog_product_service) | Object with the service fields. Empty fields are returned as `null`.

The response contains all information block properties as `propertyN` keys: a filled property is an object `{"value": ..., "valueId": ...}`, a multiple property is an array of such objects. The example shows only the `property301`, `property303`, and `property305` properties.

If `id` belongs to a product of another type, for example a simple product, `service` contains an empty array `[]` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"200040300010",
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user has none of the "View product catalog", "View catalog products", or "Edit selling price" access permissions ||
|| `400` | `200040300040` | Access Denied | The user does not have the permission to read the catalog information block element ||
|| `400` | Empty value | service does not exist. | There is no service with the specified `id`. The method returns the same error if `id` contains a string that is not a number ||
|| `400` | `100` | Could not find value for parameter {id} | The `id` parameter is not provided ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-service-add.md)
- [{#T}](./catalog-product-service-update.md)
- [{#T}](./catalog-product-service-list.md)
- [{#T}](./catalog-product-service-download.md)
- [{#T}](./catalog-product-service-delete.md)
- [{#T}](./catalog-product-service-get-fields-by-filter.md)
