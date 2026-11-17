# Get Available Property Parameters v 3.0 catalog.product.property.feature.available.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" access permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.product.property.feature.available.list` method returns the parameters that can be assigned to a property. The set depends on the property type and the catalog: for example, `OFFER_TREE` is available only for variation properties.

The method does not show which parameters the property already has or whether they are enabled. These are returned by the [catalog.product.property.feature.list](./catalog-product-property-feature-list-rest-v3.md) method.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../data-types.md) | A single condition on the property identifier `propertyId`: `[["propertyId", "=", 907]]`. Other fields and the `in`, `>`, `!=` operators are not supported.

You can retrieve the property identifier with the [catalog.product.property.list](../product-property/catalog-product-property-list-rest-v3.md) method ||
|| **select**
[`array`](../../data-types.md) | Array of fields to return in the response.

Available fields:

- `moduleId` — module identifier
- `featureId` — parameter code
- `featureName` — parameter name

Default: all fields ||
|| **pagination**
[`object`](../../data-types.md) | Pagination object. Without it, the method returns up to 50 parameters.

- `limit` — number of parameters per page, default `50`
- `page` — page number, starting from `1`
- `offset` — number of parameters to skip ||
|#

Sorting with `order` is not supported.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.property.feature.available.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["propertyId","=",907]]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.property.feature.available.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["propertyId","=",907]],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.property.feature.available.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type AvailableFeatureListResult = {
      items: Array<{
        moduleId: string
        featureId: string
        featureName: string
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<AvailableFeatureListResult>({
        method: 'catalog.product.property.feature.available.list',
        params: {
            filter: [
              ['propertyId', '=', 907],
            ],
          },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Available features:', result.items.map((feature) => feature.featureId))
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
      async function listAvailableFeatures() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.property.feature.available.list',
            params: {
                  filter: [
                    ['propertyId', '=', 907],
                  ],
                },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Available features:', result.items.map((feature) => feature.featureId))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listAvailableFeatures)
    </script>
    ```

- Python

    The `catalog.product.property.feature.*` REST 3.0 methods have no ready-made wrappers in `b24pysdk`, and the SDK sends to REST 3.0 only the methods listed in `Config().api_v3_methods`. Add the method to this list and call it with `call_method` of the client token, passing `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "catalog.product.property.feature.available.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})
    bitrix_token = client.get_token()

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "filter": [
                    ["propertyId", "=", 907],
                ],
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"code: {error.code}",
            f"message: {error.error.message}",
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
                    'catalog.product.property.feature.available.list',
                    [
                        'filter' => [
                            ['propertyId', '=', 907],
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
        'catalog.product.property.feature.available.list',
        {
            filter: [
                ['propertyId', '=', 907],
            ],
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
        'catalog.product.property.feature.available.list',
        [
            'filter' => [
                ['propertyId', '=', 907],
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
    res, err := client.Core().Call(ctx, "catalog.product.property.feature.available.list", b24.Params{
    	"filter": []any{
    		[]any{"propertyId", "=", 907},
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.property.feature.available.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ModuleID    string `json:"moduleId"`
    		FeatureID   string `json:"featureId"`
    		FeatureName string `json:"featureName"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ModuleID, item.FeatureID, item.FeatureName)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "moduleId": "iblock",
                "featureId": "LIST_PAGE_SHOW",
                "featureName": "Show on items page"
            },
            {
                "moduleId": "iblock",
                "featureId": "DETAIL_PAGE_SHOW",
                "featureName": "Show on item details page"
            },
            {
                "moduleId": "catalog",
                "featureId": "IN_BASKET",
                "featureName": "Add value to shopping cart"
            },
            {
                "moduleId": "catalog",
                "featureId": "OFFER_TREE",
                "featureName": "Can select SKU's"
            }
        ]
    },
    "time": {
        "start": 1794923353,
        "finish": 1794923353.848634,
        "duration": 0.8486340045928955,
        "processing": 0,
        "date_start": "2026-11-17T16:49:13+02:00",
        "date_finish": "2026-11-17T16:49:13+02:00",
        "operating_reset_at": 1794923940,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the response data ||
|| **result.items**
[`array`](../../data-types.md) | Array of available parameters. The set of fields depends on `select`. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#item}

#|
|| **Name**
`type` | **Description** ||
|| **moduleId**
[`string`](../../data-types.md) | Identifier of the module to which the parameter belongs: `iblock` or `catalog` ||
|| **featureId**
[`string`](../../data-types.md) | Parameter code ||
|| **featureName**
[`string`](../../data-types.md) | Parameter name in the Bitrix24 language ||
|#

Possible combinations of `moduleId` and `featureId`:

- `iblock` and `LIST_PAGE_SHOW` — show the property in the product list
- `iblock` and `DETAIL_PAGE_SHOW` — show the property on the product details page
- `catalog` and `IN_BASKET` — add the property value to the shopping cart
- `catalog` and `OFFER_TREE` — select variations by the property

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION",
        "message": "Error validating request object filters.",
        "validation": [
            {
                "message": "Filter for required field \"propertyId\" is required.",
                "field": "filter.propertyId"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Field value must not be empty | Pass `filter` with a condition on `propertyId` ||
|| `#FIELD#` | DTO `ProductPropertyAvailableFeatureDto` in field `#FIELD#` requires attribute `Filterable` to perform this request | Filter only by `propertyId` ||
|| `#FIELD#` | DTO `ProductPropertyAvailableFeatureDto` in field `#FIELD#` requires attribute `Sortable` to perform this request | Remove `order` from the request ||
|#

#### Property Filter Error

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.propertyId` | Filter for required field `propertyId` is required | Pass a single condition `["propertyId", "=", <id>]` without `in` or comparison operators ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `propertyId` | Entity with ID `#ID#` was not found | The property does not exist or does not belong to the trade catalog. Check the identifier with the [catalog.product.property.list](../product-property/catalog-product-property-list-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `ProductPropertyAvailableFeatureDto` | Pass in `select` only the fields from the `select` parameter list ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `#PAGINATION#` | Pass a positive number in `limit` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check that the user has the "View product catalog" access permission ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-feature-add-rest-v3.md)
- [{#T}](./catalog-product-property-feature-list-rest-v3.md)
- [{#T}](./catalog-product-property-feature-available-field-list.md)
