# Get a List of Product or Variation Properties v 3.0 catalog.product.property.list

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

The `catalog.product.property.list` method returns a list of product and variation properties. Properties of information blocks that are not trade catalogs, such as universal lists, are not included in the list.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | List of property fields to return in the response.

Available fields:

- `id` — property identifier
- `timestampX` — date and time of the last modification
- `iblockId` — trade catalog identifier
- `name` — name
- `active` — activity
- `sort` — sort order
- `code` — symbolic code
- `defaultValue` — default value
- `propertyType` — base type
- `userType` — custom type
- `userTypeSettings` — custom type settings
- `rowCount` — number of rows in the input field
- `colCount` — number of columns in the input field
- `listType` — list appearance
- `multiple` — multiple property
- `multipleCnt` — number of fields for multiple values
- `xmlId` — external identifier
- `fileType` — file extensions
- `linkIblockId` — linked information block identifier
- `withDescription` — whether a description can be added to a value
- `searchable` — participation in search
- `filtrable` — participation in filtering
- `isRequired` — whether the property is required
- `hint` — hint
- `controlId` — name of the property field in information block forms

Default: all fields ||
|| **filter**
[`array`](../../data-types.md) | Filter in the REST 3.0 format: an array of conditions like `["field", "operator", "value"]`, for example `[["iblockId","=",25]]`. The filter syntax is described in the [REST 3.0 overview](../../rest-v3.md#filter).

You can filter by the `id`, `timestampX`, `iblockId`, `name`, `active`, `sort`, `code`, `propertyType`, `userType`, `multiple`, `xmlId`, `linkIblockId`, `withDescription`, `searchable`, `filtrable`, and `isRequired` fields. For flags, pass `true` or `false`.

Negation by `iblockId` is not supported: instead of an "all catalogs except one" condition, list the required catalogs using the `in` operator.

Trade catalog identifiers can be retrieved using the [catalog.catalog.list](../catalog/catalog-catalog-list-rest-v3.md) method ||
|| **order**
[`object`](../../data-types.md) | Sorting in the `{"field": "direction"}` format, for example `{"sort":"ASC"}`.

You can sort by the `id`, `timestampX`, `iblockId`, `name`, `active`, `sort`, `code`, `propertyType`, and `xmlId` fields.

Possible direction values:

- `ASC` — ascending
- `DESC` — descending ||
|| **pagination**
[`object`](../../data-types.md) | Pagination. [Object description](#pagination) ||
|#

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../data-types.md) | Number of properties per page. Values greater than `1000` are reduced to `1000`. Zero and negative values cause an error.

Default: `50` ||
|| **page**
[`integer`](../../data-types.md) | Page number, starting from `1`. If `page` is passed, the `offset` value is ignored ||
|| **offset**
[`integer`](../../data-types.md) | Number of properties to skip from the beginning of the list.

Default: `0` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.property.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","iblockId","name","code","propertyType","userType","multiple"],"filter":[["iblockId","=",25],["propertyType","=","L"]],"order":{"sort":"ASC"},"pagination":{"limit":3}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.property.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","iblockId","name","code","propertyType","userType","multiple"],"filter":[["iblockId","=",25],["propertyType","=","L"]],"order":{"sort":"ASC"},"pagination":{"limit":3},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.property.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProductPropertyListResult = {
      items: Array<{
        id: number
        iblockId: number
        name: string
        code: string | null
        propertyType: string
        userType: string | null
        multiple: boolean
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<ProductPropertyListResult>({
        method: 'catalog.product.property.list',
        params: {
          select: ['id', 'iblockId', 'name', 'code', 'propertyType', 'userType', 'multiple'],
          filter: [
            ['iblockId', '=', 25],
            ['propertyType', '=', 'L'],
          ],
          order: {
            sort: 'ASC',
          },
          pagination: {
            limit: 3,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.items.forEach(property => console.info(property.id, property.name, property.code))
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
      async function listProductProperties() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.property.list',
            params: {
              select: ['id', 'iblockId', 'name', 'code', 'propertyType', 'userType', 'multiple'],
              filter: [
                ['iblockId', '=', 25],
                ['propertyType', '=', 'L'],
              ],
              order: {
                sort: 'ASC',
              },
              pagination: {
                limit: 3,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.items.forEach(property => console.info(property.id, property.name, property.code))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listProductProperties)
    </script>
    ```

- Python

    The `catalog.product.property.*` REST 3.0 methods have no ready-made wrappers in `b24pysdk`, and the SDK sends to REST 3.0 only the methods listed in `Config().api_v3_methods`. Add the method to this list and call it with `call_method` of the client token, passing `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "catalog.product.property.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})
    bitrix_token = client.get_token()

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "select": ["id", "iblockId", "name", "code", "propertyType", "userType", "multiple"],
                "filter": [
                    ["iblockId", "=", 25],
                    ["propertyType", "=", "L"],
                ],
                "order": {
                    "sort": "ASC",
                },
                "pagination": {
                    "limit": 3,
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
                'catalog.product.property.list',
                [
                    'select' => ['id', 'iblockId', 'name', 'code', 'propertyType', 'userType', 'multiple'],
                    'filter' => [
                        ['iblockId', '=', 25],
                        ['propertyType', '=', 'L'],
                    ],
                    'order' => [
                        'sort' => 'ASC',
                    ],
                    'pagination' => [
                        'limit' => 3,
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
        'catalog.product.property.list',
        {
            select: ['id', 'iblockId', 'name', 'code', 'propertyType', 'userType', 'multiple'],
            filter: [
                ['iblockId', '=', 25],
                ['propertyType', '=', 'L'],
            ],
            order: {
                sort: 'ASC',
            },
            pagination: {
                limit: 3,
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
        'catalog.product.property.list',
        [
            'select' => ['id', 'iblockId', 'name', 'code', 'propertyType', 'userType', 'multiple'],
            'filter' => [
                ['iblockId', '=', 25],
                ['propertyType', '=', 'L'],
            ],
            'order' => [
                'sort' => 'ASC',
            ],
            'pagination' => [
                'limit' => 3,
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
    res, err := client.Core().Call(ctx, "catalog.product.property.list", b24.Params{
    	"select": []string{"id", "iblockId", "name", "code", "propertyType", "userType", "multiple"},
    	"filter": []any{
    		[]any{"iblockId", "=", 25},
    		[]any{"propertyType", "=", "L"},
    	},
    	"order": b24.Params{
    		"sort": "ASC",
    	},
    	"pagination": b24.Params{
    		"limit": 3,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.property.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID           int    `json:"id"`
    		Name         string `json:"name"`
    		Code         string `json:"code"`
    		PropertyType string `json:"propertyType"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, property := range result.Items {
    	fmt.Println(property.ID, property.Name, property.Code, property.PropertyType)
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
                "id": 295,
                "iblockId": 25,
                "name": "New",
                "code": "NEWPRODUCT",
                "propertyType": "L",
                "userType": null,
                "multiple": false
            },
            {
                "id": 297,
                "iblockId": 25,
                "name": "Bestseller",
                "code": "SALELEADER",
                "propertyType": "L",
                "userType": null,
                "multiple": false
            },
            {
                "id": 299,
                "iblockId": 25,
                "name": "Special offer",
                "code": "SPECIALOFFER",
                "propertyType": "L",
                "userType": null,
                "multiple": false
            }
        ]
    },
    "time": {
        "start": 1794922888,
        "finish": 1794922888.556874,
        "duration": 0.5568740367889404,
        "processing": 0,
        "date_start": "2026-11-17T16:41:28+02:00",
        "date_finish": "2026-11-17T16:41:28+02:00",
        "operating_reset_at": 1794923460,
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
[`array`](../../data-types.md) | Array of properties. The set of fields depends on `select`. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

The method does not return the total number of properties. If the response contains fewer properties than `limit`, the page is the last one.

#### Items Element {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Property identifier ||
|| **timestampX**
[`datetime`](../../data-types.md) | Date and time of the last property modification ||
|| **iblockId**
[`integer`](../../data-types.md) | Identifier of the trade catalog the property belongs to ||
|| **name**
[`string`](../../data-types.md) | Property name ||
|| **active**
[`boolean`](../../data-types.md) | Property activity:
- `true` — the property is active
- `false` — the property is inactive ||
|| **sort**
[`integer`](../../data-types.md) | Sort order ||
|| **code**
[`string`](../../data-types.md) or `null` | Symbolic code of the property ||
|| **defaultValue**
[`string`](../../data-types.md) or `null` | Default value ||
|| **propertyType**
[`string`](../../data-types.md) | Base property type:
- `N` — number
- `S` — string
- `L` — list
- `F` — file
- `E` — link to elements
- `G` — link to sections ||
|| **userType**
[`string`](../../data-types.md) or `null` | Custom property type, for example `Money` or `DateTime`. `null` for a property without a custom type and for a checkbox property ||
|| **userTypeSettings**
[`object`](../../data-types.md) or [`array`](../../data-types.md) | Custom type settings. If there are no settings, an empty array `[]` ||
|| **rowCount**
[`integer`](../../data-types.md) | Number of rows in the input field ||
|| **colCount**
[`integer`](../../data-types.md) | Number of columns in the input field ||
|| **listType**
[`string`](../../data-types.md) | List appearance:
- `L` — dropdown list
- `C` — checkboxes ||
|| **multiple**
[`boolean`](../../data-types.md) | Multiple property:
- `true` — the property stores multiple values
- `false` — a single value ||
|| **multipleCnt**
[`integer`](../../data-types.md) or `null` | Number of fields for entering multiple values ||
|| **xmlId**
[`string`](../../data-types.md) or `null` | External identifier of the property ||
|| **fileType**
[`string`](../../data-types.md) or `null` | File extensions for a property of type `F` ||
|| **linkIblockId**
[`integer`](../../data-types.md) or `null` | For properties of type `E` and `G` — identifier of the information block whose elements or sections the property is linked to ||
|| **withDescription**
[`boolean`](../../data-types.md) | `true` — a description can be added to the property value ||
|| **searchable**
[`boolean`](../../data-types.md) | `true` — property values are included in search ||
|| **filtrable**
[`boolean`](../../data-types.md) | `true` — the property is used in product filtering ||
|| **isRequired**
[`boolean`](../../data-types.md) | `true` — the property is required on the product card ||
|| **hint**
[`string`](../../data-types.md) or `null` | Hint for the property field ||
|| **controlId**
[`string`](../../data-types.md) | Name of the property field in information block forms in the `PROPERTY_<id>` format, for example `PROPERTY_1291` ||
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
                "message": "Filter negation over `iblockId` is not supported"
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
|| `#FIELD#` | DTO `ProductPropertyDto` in field `#FIELD#` requires attribute `Filterable` to perform this request | Filter only by the fields from the `filter` parameter description ||
|| `#FIELD#` | DTO `ProductPropertyDto` in field `#FIELD#` requires attribute `Sortable` to perform this request | Sort only by the fields from the `order` parameter description ||
|| `-` | Filter negation over `iblockId` is not supported | Remove the negation by `iblockId` and list the required catalogs using the `in` operator ||
|#

#### Filter Operator Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Unknown filter operator `#OPERATOR#` | Use the operators from the [REST 3.0 overview](../../rest-v3.md#filter) ||
|#

#### Sorting Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `order` | Invalid sort order `#ORDER#` | Pass the `ASC` or `DESC` direction ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `{"limit":0}` | Pass a number greater than `0` in `limit` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `ProductPropertyDto` | Specify only the fields from the `select` parameter description in `select` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "View product catalog" access permission ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-add-rest-v3.md)
- [{#T}](./catalog-product-property-update-rest-v3.md)
- [{#T}](./catalog-product-property-get-rest-v3.md)
- [{#T}](./catalog-product-property-delete-rest-v3.md)
- [{#T}](./catalog-product-property-field-list.md)
- [{#T}](./catalog-product-property-type-list.md)
