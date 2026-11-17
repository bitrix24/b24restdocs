# Get Product or Variation Property Parameter v 3.0 catalog.product.property.feature.get

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

The `catalog.product.property.feature.get` method returns the fields of a property parameter by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Property parameter identifier.

The identifier can be retrieved using the [catalog.product.property.feature.list](./catalog-product-property-feature-list-rest-v3.md) method ||
|| **select**
[`array`](../../data-types.md) | Array of parameter fields to return in the response.

Available fields:

- `id` — property parameter identifier
- `propertyId` — property identifier
- `moduleId` — module identifier
- `featureId` — parameter code
- `isEnabled` — parameter activity flag

The response contains only the listed fields. If you need `id`, list it as well.

Default: all fields ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.property.feature.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":211,"select":["id","propertyId","moduleId","featureId","isEnabled"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.property.feature.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":211,"select":["id","propertyId","moduleId","featureId","isEnabled"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.property.feature.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FeatureGetResult = {
      item: {
        id: number
        propertyId: number
        moduleId: string
        featureId: string
        isEnabled: boolean
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<FeatureGetResult>({
        method: 'catalog.product.property.feature.get',
        params: {
            id: 211,
            select: ['id', 'propertyId', 'moduleId', 'featureId', 'isEnabled'],
          },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Feature:', result.item.featureId, result.item.isEnabled)
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
      async function getPropertyFeature() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.property.feature.get',
            params: {
                  id: 211,
                  select: ['id', 'propertyId', 'moduleId', 'featureId', 'isEnabled'],
                },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Feature:', result.item.featureId, result.item.isEnabled)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getPropertyFeature)
    </script>
    ```

- Python

    The `catalog.product.property.feature.*` REST 3.0 methods have no ready-made wrappers in `b24pysdk`, and the SDK sends to REST 3.0 only the methods listed in `Config().api_v3_methods`. Add the method to this list and call it with `call_method` of the client token, passing `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "catalog.product.property.feature.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})
    bitrix_token = client.get_token()

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 211,
                "select": ["id", "propertyId", "moduleId", "featureId", "isEnabled"],
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
                    'catalog.product.property.feature.get',
                    [
                        'id' => 211,
                        'select' => ['id', 'propertyId', 'moduleId', 'featureId', 'isEnabled'],
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
        'catalog.product.property.feature.get',
        {
            id: 211,
            select: ['id', 'propertyId', 'moduleId', 'featureId', 'isEnabled'],
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
        'catalog.product.property.feature.get',
        [
            'id' => 211,
            'select' => ['id', 'propertyId', 'moduleId', 'featureId', 'isEnabled'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.product.property.feature.get", b24.Params{
    	"id":     211,
    	"select": []string{"id", "propertyId", "moduleId", "featureId", "isEnabled"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.property.feature.get: %w", err)
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
            "id": 211,
            "propertyId": 1285,
            "moduleId": "catalog",
            "featureId": "IN_BASKET",
            "isEnabled": true
        }
    },
    "time": {
        "start": 1794923350,
        "finish": 1794923350.505961,
        "duration": 0.5059609413146973,
        "processing": 0,
        "date_start": "2026-11-17T16:49:10+02:00",
        "date_finish": "2026-11-17T16:49:10+02:00",
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
[`object`](../../data-types.md) | Parameter fields. The set of fields depends on `select`. [Field description](#item) ||
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
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 999999 was not found."
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass a positive number in `id` ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | Check the identifier using the [catalog.product.property.feature.list](./catalog-product-property-feature-list-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `ProductPropertyFeatureDto` | Pass only the fields from the `select` parameter list in `select` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "View product catalog" access permission ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-feature-add-rest-v3.md)
- [{#T}](./catalog-product-property-feature-update-rest-v3.md)
- [{#T}](./catalog-product-property-feature-list-rest-v3.md)
- [{#T}](./catalog-product-property-feature-available-list.md)
- [{#T}](./catalog-product-property-feature-field-list.md)
