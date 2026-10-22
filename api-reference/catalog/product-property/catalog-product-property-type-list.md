# Get List of Product Property Types catalog.product.property.type.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.product.property.type.list` method returns the product and variation property types supported by the trade catalog.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | The list of property type fields to return in the response.

Available fields:

- `id` — property type code
- `propertyType` — base property type
- `userType` — custom property type

Default: all fields. An empty array `[]` also returns all fields ||
|#

The method returns the entire list at once. The `filter`, `order`, and `pagination` parameters are not supported.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.property.type.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","propertyType","userType"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.property.type.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","propertyType","userType"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.property.type.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProductPropertyTypeListResult = {
      items: Array<{
        id: string
        propertyType: string
        userType: string | null
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<ProductPropertyTypeListResult>({
        method: 'catalog.product.property.type.list',
        params: {
          select: [
            'id',
            'propertyType',
            'userType',
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.items.forEach(type => console.info(type.id, type.propertyType, type.userType))
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
      async function listProductPropertyTypes() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.property.type.list',
            params: {
              select: [
                'id',
                'propertyType',
                'userType',
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.items.forEach(type => console.info(type.id, type.propertyType, type.userType))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listProductPropertyTypes)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.property.type.list(
            select=["id", "propertyType", "userType"],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.product.property.type.list',
                [
                    'select' => ['id', 'propertyType', 'userType'],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'catalog.product.property.type.list',
        {
            select: ['id', 'propertyType', 'userType'],
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
        'catalog.product.property.type.list',
        [
            'select' => ['id', 'propertyType', 'userType'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.product.property.type.list", b24.Params{
    	"select": []string{"id", "propertyType", "userType"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.property.type.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID           string  `json:"id"`
    		PropertyType string  `json:"propertyType"`
    		UserType     *string `json:"userType"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, t := range result.Items {
    	fmt.Println(t.ID, t.PropertyType)
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
                "id": "NUMBER",
                "propertyType": "N",
                "userType": null
            },
            {
                "id": "STRING",
                "propertyType": "S",
                "userType": null
            },
            {
                "id": "LIST",
                "propertyType": "L",
                "userType": null
            },
            {
                "id": "BOOL_ENUM",
                "propertyType": "L",
                "userType": "BoolEnum"
            },
            {
                "id": "DATETIME",
                "propertyType": "S",
                "userType": "DateTime"
            },
            {
                "id": "MONEY",
                "propertyType": "S",
                "userType": "Money"
            },
            {
                "id": "SKU",
                "propertyType": "E",
                "userType": "SKU"
            }
        ]
    },
    "time": {
        "start": 1792660566,
        "finish": 1792660566.778289,
        "duration": 0.7782890796661377,
        "processing": 0,
        "date_start": "2026-10-22T12:16:06+02:00",
        "date_finish": "2026-10-22T12:16:06+02:00",
        "operating_reset_at": 1792661160,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | An object with the response data ||
|| **result.items**
[`array`](../../data-types.md) | An array of property types. The set of fields depends on `select`. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Property type code.

Possible values:

- `NUMBER` — number
- `STRING` — string
- `LIST` — list
- `BOOL_ENUM` — "Yes/No" checkbox
- `DATETIME` — date and time
- `MONEY` — money
- `SKU` — binding of a variation to a product ||
|| **propertyType**
[`string`](../../data-types.md) | Base type of the information block property.

Possible values:

- `N` — number
- `S` — string
- `L` — list
- `E` — binding to elements ||
|| **userType**
[`string`](../../data-types.md) or `null` | Custom property type that refines the base type: `BoolEnum`, `DateTime`, `Money`, `SKU`. `null` — a property of the base type ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION",
        "message": "Incorrect \"pagination\" parameter value: {\"limit\":2}."
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `#FIELD#` | DTO `ProductPropertyTypeDto` in field `#FIELD#` requires attribute `Filterable` to perform this request | Remove `filter` from the request: the method returns the entire list ||
|| `#FIELD#` | DTO `ProductPropertyTypeDto` in field `#FIELD#` requires attribute `Sortable` to perform this request | Remove `order` from the request ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `#PAGINATION#` | Remove `pagination` from the request: the method returns the entire list ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `ProductPropertyTypeDto` | Specify in `select` only the fields from the `select` parameter description. The `["*"]` value is not supported ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-type-field-list.md)
- [{#T}](./catalog-product-property-add-rest-v3.md)
- [{#T}](./catalog-product-property-field-list.md)
