# Update Product Property or Variation Parameter catalog.productPropertyFeature.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View Product Catalog" permission and the right to modify the information block property.

The method `catalog.productPropertyFeature.update` updates the parameter of a product or variation property.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the property parameter.

The parameter identifier can be obtained using the [catalog.productPropertyFeature.list](./catalog-product-property-feature-list.md) method. ||
|| **fields***
[`object`](../../data-types.md) | Set of fields for the updated property parameter [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **propertyId***
[`catalog_product_property.id`](../data-types.md#catalog_product_property) | Identifier of the product or variation property.

Property identifiers can be obtained using the [catalog.productProperty.list](../product-property/catalog-product-property-list.md) method. ||
|| **moduleId***
[`string`](../../data-types.md) | Identifier of the module to which the property parameter belongs.

The module identifier can be obtained using the [catalog.productPropertyFeature.getAvailableFeaturesByProperty](./catalog-product-property-feature-get-available-features-by-property.md) method. ||
|| **featureId***
[`string`](../../data-types.md) | Code of the property parameter.

The parameter code can be obtained using the [catalog.productPropertyFeature.getAvailableFeaturesByProperty](./catalog-product-property-feature-get-available-features-by-property.md) method. ||
|| **isEnabled***
[`char`](../../data-types.md) | Indicator of the parameter's activity. Acceptable values:
- `Y` — enabled
- `N` — disabled ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"id":101,"fields":{"propertyId":901,"moduleId":"iblock","featureId":"LIST_PAGE_SHOW","isEnabled":"N"}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productPropertyFeature.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"id":101,"fields":{"propertyId":901,"moduleId":"iblock","featureId":"LIST_PAGE_SHOW","isEnabled":"N"},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/catalog.productPropertyFeature.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type UpdateProductPropertyFeatureResult = {
      productPropertyFeature: {
        featureId: string
        id: number
        isEnabled: string
        moduleId: string
        propertyId: number
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<UpdateProductPropertyFeatureResult>({
        method: 'catalog.productPropertyFeature.update',
        params: {
          id: 101,
          fields: {
            propertyId: 901,
            moduleId: 'iblock',
            featureId: 'LIST_PAGE_SHOW',
            isEnabled: 'N',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.productPropertyFeature)
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
      async function updateProductPropertyFeature() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.productPropertyFeature.update',
            params: {
              id: 101,
              fields: {
                propertyId: 901,
                moduleId: 'iblock',
                featureId: 'LIST_PAGE_SHOW',
                isEnabled: 'N',
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
          console.info(result.productPropertyFeature)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateProductPropertyFeature)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.productPropertyFeature.update',
                [
                    'id' => 101,
                    'fields' => [
                        'propertyId' => 901,
                        'moduleId' => 'iblock',
                        'featureId' => 'LIST_PAGE_SHOW',
                        'isEnabled' => 'N',
                    ],
                ]
            );

        print_r($response->getResponseData()->getResult());
    } catch (\Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.productPropertyFeature.update',
        {
            id: 101,
            fields: {
                propertyId: 901,
                moduleId: 'iblock',
                featureId: 'LIST_PAGE_SHOW',
                isEnabled: 'N',
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.productPropertyFeature.update',
        [
            'id' => 101,
            'fields' => [
                'propertyId' => 901,
                'moduleId' => 'iblock',
                'featureId' => 'LIST_PAGE_SHOW',
                'isEnabled' => 'N',
            ]
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "productPropertyFeature": {
        "featureId": "LIST_PAGE_SHOW",
        "id": 101,
        "isEnabled": "N",
        "moduleId": "iblock",
        "propertyId": 901
        }
    },
    "time": {
        "start": 1774015479,
        "finish": 1774015479.592042,
        "duration": 0.5920419692993164,
        "processing": 0,
        "date_start": "2026-03-20T17:04:39+01:00",
        "date_finish": "2026-03-20T17:04:39+01:00",
        "operating_reset_at": 1774016079,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root object of the response ||
|| **productPropertyFeature**
[`catalog_product_property_features`](../data-types.md#catalog_product_property_features) | Object of the updated property parameter ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "productPropertyFeature does not exist."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Access Denied | Insufficient permissions to modify the information block property ||
|| `0` | productPropertyFeature does not exist. | The property parameter with the provided `id` was not found or the `propertyId` does not belong to the product catalog ||
|| `0` | Required fields: moduleId, featureId, isEnabled | Required fields `moduleId`, `featureId`, `isEnabled` were not provided ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-feature-add.md)
- [{#T}](./catalog-product-property-feature-get.md)
- [{#T}](./catalog-product-property-feature-list.md)
- [{#T}](./catalog-product-property-feature-get-available-features-by-property.md)
- [{#T}](./catalog-product-property-feature-get-fields.md)