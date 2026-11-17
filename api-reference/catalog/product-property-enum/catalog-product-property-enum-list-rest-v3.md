# Get a List of Values for List Properties v 3.0 catalog.product.property.enum.list

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

The `catalog.product.property.enum.list` method returns values of "List" properties by filter. The response includes only values of trade catalog properties.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | Array of value fields to return in the response.

Available fields:

- `id` — value identifier
- `propertyId` — property identifier
- `value` — value text
- `def` — default value flag
- `sort` — sort index
- `xmlId` — value external ID

The response includes only the listed fields. If you need `id`, list it too.

Default: all fields ||
|| **filter**
[`array`](../../data-types.md) | Filter in the REST 3.0 format: an array of conditions like `["field", "operator", "value"]`. You can filter by all fields from `select`.

To retrieve the values of a single property, pass a condition on `propertyId`. For several properties, list them with the `in` operator. Negation on `propertyId` is not supported: the condition `["propertyId", "!=", 295]` returns an error.

For `def`, pass `true` or `false`.

The syntax and operators are described in the [REST 3.0 overview](../../rest-v3.md#filter) ||
|| **order**
[`object`](../../data-types.md) | Sorting in the `{"field": "direction"}` format. You can sort by the `id`, `propertyId`, `value`, `sort`, `xmlId` fields. Sorting by `def` causes an error.

Possible direction values:

- `ASC` — ascending
- `DESC` — descending ||
|| **pagination**
[`object`](../../data-types.md) | Pagination object. [Object structure description](#pagination) ||
|#

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../data-types.md) | Number of values per page. Values greater than `1000` are reduced to `1000`. Zero causes an error.

Default: `50` ||
|| **page**
[`integer`](../../data-types.md) | Page number, starting from `1`. The offset is calculated as `(page - 1) × limit` ||
|| **offset**
[`integer`](../../data-types.md) | Number of values to skip from the beginning of the list.

Default: `0` ||
|#

{% note info "" %}

The response does not contain the total number of values. If the page returns fewer values than specified in `limit`, it is the last page.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.property.enum.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","value","def","sort","xmlId"],"filter":[["propertyId","=",1285]],"order":{"sort":"ASC"},"pagination":{"limit":3}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.property.enum.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","value","def","sort","xmlId"],"filter":[["propertyId","=",1285]],"order":{"sort":"ASC"},"pagination":{"limit":3},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.property.enum.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type EnumListResult = {
      items: Array<{
        id: number
        value: string
        def: boolean
        sort: number
        xmlId: string
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<EnumListResult>({
        method: 'catalog.product.property.enum.list',
        params: {
            select: ['id', 'value', 'def', 'sort', 'xmlId'],
            filter: [
              ['propertyId', '=', 1285],
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
        console.info('Values:', result.items.length)
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
      async function listEnumValues() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.property.enum.list',
            params: {
                  select: ['id', 'value', 'def', 'sort', 'xmlId'],
                  filter: [
                    ['propertyId', '=', 1285],
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
          console.info('Values:', result.items.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listEnumValues)
    </script>
    ```

- Python

    The `catalog.product.property.enum.*` REST 3.0 methods have no ready-made wrappers in `b24pysdk`, and the SDK sends to REST 3.0 only the methods listed in `Config().api_v3_methods`. Add the method to this list and call it with `call_method` of the client token, passing `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "catalog.product.property.enum.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})
    bitrix_token = client.get_token()

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "select": ["id", "value", "def", "sort", "xmlId"],
                "filter": [
                    ["propertyId", "=", 1285],
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
                    'catalog.product.property.enum.list',
                    [
                        'select' => ['id', 'value', 'def', 'sort', 'xmlId'],
                        'filter' => [
                            ['propertyId', '=', 1285],
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
        'catalog.product.property.enum.list',
        {
            select: ['id', 'value', 'def', 'sort', 'xmlId'],
            filter: [
                ['propertyId', '=', 1285],
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
        'catalog.product.property.enum.list',
        [
            'select' => ['id', 'value', 'def', 'sort', 'xmlId'],
            'filter' => [
                ['propertyId', '=', 1285],
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
    res, err := client.Core().Call(ctx, "catalog.product.property.enum.list", b24.Params{
    	"select": []string{"id", "value", "def", "sort", "xmlId"},
    	"filter": []any{
    		[]any{"propertyId", "=", 1285},
    	},
    	"order": b24.Params{
    		"sort": "ASC",
    	},
    	"pagination": b24.Params{
    		"limit": 3,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.property.enum.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID    int    `json:"id"`
    		Value string `json:"value"`
    		Def   bool   `json:"def"`
    		Sort  int    `json:"sort"`
    		XMLID string `json:"xmlId"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.Value, item.Def)
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
                "id": 1779,
                "value": "Red",
                "def": true,
                "sort": 100,
                "xmlId": "red"
            },
            {
                "id": 1781,
                "value": "Navy blue",
                "def": false,
                "sort": 200,
                "xmlId": "blue"
            },
            {
                "id": 1789,
                "value": "Lemon",
                "def": false,
                "sort": 350,
                "xmlId": "color-yellow"
            }
        ]
    },
    "time": {
        "start": 1794922619,
        "finish": 1794922619.386468,
        "duration": 0.38646793365478516,
        "processing": 0,
        "date_start": "2026-11-17T16:36:59+02:00",
        "date_finish": "2026-11-17T16:36:59+02:00",
        "operating_reset_at": 1794923160,
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
[`array`](../../data-types.md) | Array of values on the page. The set of fields depends on `select`. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Value identifier ||
|| **propertyId**
[`integer`](../../data-types.md) | Identifier of the property the value belongs to ||
|| **value**
[`string`](../../data-types.md) | Value text ||
|| **def**
[`boolean`](../../data-types.md) | Default value flag: `true` — default value, `false` — regular value ||
|| **sort**
[`integer`](../../data-types.md) | Sort index ||
|| **xmlId**
[`string`](../../data-types.md) | Value external ID ||
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
                "message": "Filter negation over `propertyId` is not supported"
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
|| `-` | Filter negation over `propertyId` is not supported | Remove the negation on `propertyId` from the filter. List the required properties with the `in` operator ||
|| `def` | DTO `ProductPropertyEnumDto` in field `def` requires attribute `Sortable` to perform this request | Remove `def` from `order` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select`, `filter`, `order` | Unknown field `#FIELD#` found in entity `ProductPropertyEnumDto` | Use only the fields from the `select` parameter list ||
|#

#### Filter Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `Unknown filter condition` | Pass `filter` as an array of conditions, for example `[["propertyId","=",1285]]` ||
|#

#### Filter Operator Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Unknown filter operator `#OPERATOR#` | Use the operators from the [REST 3.0 overview](../../rest-v3.md#operators) ||
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

- [{#T}](./catalog-product-property-enum-add-rest-v3.md)
- [{#T}](./catalog-product-property-enum-update-rest-v3.md)
- [{#T}](./catalog-product-property-enum-get-rest-v3.md)
- [{#T}](./catalog-product-property-enum-delete-rest-v3.md)
- [{#T}](./catalog-product-property-enum-field-list.md)
