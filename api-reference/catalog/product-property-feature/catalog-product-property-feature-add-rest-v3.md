# Add Product or Variation Property Parameter v 3.0 catalog.product.property.feature.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" access permission and permission to edit the property's information block

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.product.property.feature.add` method adds a parameter to a product or variation property and returns its fields. A parameter defines how the property is used in the catalog: for example, whether it is shown in the product list and whether its value is added to the shopping cart.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Object with the fields of the new parameter. [Field description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **propertyId***
[`integer`](../../data-types.md) | Identifier of the product catalog property.

The identifier can be retrieved using the [catalog.product.property.list](../product-property/catalog-product-property-list-rest-v3.md) method ||
|| **moduleId***
[`string`](../../data-types.md) | Identifier of the module the parameter belongs to: `iblock` or `catalog` ||
|| **featureId***
[`string`](../../data-types.md) | Parameter code.

The [catalog.product.property.feature.available.list](./catalog-product-property-feature-available-list.md) method returns the `moduleId` and `featureId` combinations available to the property. Possible combinations:

- `iblock` and `LIST_PAGE_SHOW` — show the property in the product list
- `iblock` and `DETAIL_PAGE_SHOW` — show the property on the product details page
- `catalog` and `IN_BASKET` — add the property value to the shopping cart
- `catalog` and `OFFER_TREE` — select variations by the property ||
|| **isEnabled***
[`boolean`](../../data-types.md) | Parameter activity flag.

Possible values:

- `true` — the parameter is enabled
- `false` — the parameter is disabled

The strings `Y` and `N` are not accepted ||
|#

Each `moduleId` and `featureId` combination can be added to a property only once. Bitrix24 fills in the `id` field automatically, so do not pass it.

{% note warning "" %}

Before adding a parameter, check that it is available to the property using the [catalog.product.property.feature.available.list](./catalog-product-property-feature-available-list.md) method. The `catalog.product.property.feature.add` method does not check this and stores any `moduleId` and `featureId`, even nonexistent ones. Such a parameter cannot be deleted.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.property.feature.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"propertyId":1285,"moduleId":"iblock","featureId":"DETAIL_PAGE_SHOW","isEnabled":true}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.property.feature.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"propertyId":1285,"moduleId":"iblock","featureId":"DETAIL_PAGE_SHOW","isEnabled":true},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.property.feature.add
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FeatureAddResult = {
      item: {
        id: number
        propertyId: number
        moduleId: string
        featureId: string
        isEnabled: boolean
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<FeatureAddResult>({
        method: 'catalog.product.property.feature.add',
        params: {
            fields: {
              propertyId: 1285,
              moduleId: 'iblock',
              featureId: 'DETAIL_PAGE_SHOW',
              isEnabled: true,
            },
          },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Feature created:', result.item.id)
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
      async function addPropertyFeature() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.property.feature.add',
            params: {
                  fields: {
                    propertyId: 1285,
                    moduleId: 'iblock',
                    featureId: 'DETAIL_PAGE_SHOW',
                    isEnabled: true,
                  },
                },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Feature created:', result.item.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addPropertyFeature)
    </script>
    ```

- Python

    The `catalog.product.property.feature.*` REST 3.0 methods have no ready-made wrappers in `b24pysdk`, and the SDK sends to REST 3.0 only the methods listed in `Config().api_v3_methods`. Add the method to this list and call it with `call_method` of the client token, passing `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "catalog.product.property.feature.add"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})
    bitrix_token = client.get_token()

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "fields": {
                    "propertyId": 1285,
                    "moduleId": "iblock",
                    "featureId": "DETAIL_PAGE_SHOW",
                    "isEnabled": True,
                },
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
                    'catalog.product.property.feature.add',
                    [
                        'fields' => [
                            'propertyId' => 1285,
                            'moduleId' => 'iblock',
                            'featureId' => 'DETAIL_PAGE_SHOW',
                            'isEnabled' => true,
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
        'catalog.product.property.feature.add',
        {
            fields: {
                propertyId: 1285,
                moduleId: 'iblock',
                featureId: 'DETAIL_PAGE_SHOW',
                isEnabled: true,
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
        'catalog.product.property.feature.add',
        [
            'fields' => [
                'propertyId' => 1285,
                'moduleId' => 'iblock',
                'featureId' => 'DETAIL_PAGE_SHOW',
                'isEnabled' => true,
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
    res, err := client.Core().Call(ctx, "catalog.product.property.feature.add", b24.Params{
    	"fields": b24.Params{
    		"propertyId": 1285,
    		"moduleId":   "iblock",
    		"featureId":  "DETAIL_PAGE_SHOW",
    		"isEnabled":  true,
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.product.property.feature.add: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var item struct {
    	ID         int    `json:"id"`
    	PropertyID int    `json:"propertyId"`
    	ModuleID   string `json:"moduleId"`
    	FeatureID  string `json:"featureId"`
    	IsEnabled  bool   `json:"isEnabled"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.FeatureID, item.IsEnabled)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 221,
            "propertyId": 1285,
            "moduleId": "iblock",
            "featureId": "DETAIL_PAGE_SHOW",
            "isEnabled": true
        }
    },
    "time": {
        "start": 1794923346,
        "finish": 1794923346.936147,
        "duration": 0.9361469745635986,
        "processing": 0,
        "date_start": "2026-11-17T16:49:06+02:00",
        "date_finish": "2026-11-17T16:49:06+02:00",
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
|| **result.item**
[`object`](../../data-types.md) | Fields of the created parameter. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Property parameter identifier ||
|| **propertyId**
[`integer`](../../data-types.md) | Identifier of the property the parameter belongs to ||
|| **moduleId**
[`string`](../../data-types.md) | Identifier of the module the parameter belongs to: `iblock` or `catalog` ||
|| **featureId**
[`string`](../../data-types.md) | Parameter code, for example `LIST_PAGE_SHOW` or `IN_BASKET` ||
|| **isEnabled**
[`boolean`](../../data-types.md) | Parameter activity flag: `true` — the parameter is enabled, `false` — disabled ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "The feature is already assigned to the property"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `#FIELD#` | Required field "#FIELD#" is empty | Pass the `propertyId`, `moduleId`, `featureId`, and `isEnabled` fields in `fields` ||
|| `id` | Field "id" is read-only | Remove `id` from `fields` ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `-` | The feature is already assigned to the property | The property already has this parameter. Find it using the [catalog.product.property.feature.list](./catalog-product-property-feature-list-rest-v3.md) method and update it using the [catalog.product.property.feature.update](./catalog-product-property-feature-update-rest-v3.md) method ||
|| `-` | Property #ID# does not exist | Check `propertyId` using the [catalog.product.property.list](../product-property/catalog-product-property-list-rest-v3.md) method ||
|| `-` | The specified property does not belong to a product catalog | Specify a product catalog property. Catalog properties are returned by the [catalog.product.property.list](../product-property/catalog-product-property-list-rest-v3.md) method ||
|| `isEnabled` | This request requires data type `bool` in field `isEnabled` | Pass `true` or `false`, not `Y` or `N` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `ProductPropertyFeatureDto` | Pass only the fields from the `fields` parameter table in `fields` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "View product catalog" access permission and permission to edit the property's information block ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-feature-update-rest-v3.md)
- [{#T}](./catalog-product-property-feature-get-rest-v3.md)
- [{#T}](./catalog-product-property-feature-list-rest-v3.md)
- [{#T}](./catalog-product-property-feature-available-list.md)
- [{#T}](./catalog-product-property-feature-field-list.md)
