# Get the List of Product Items of an Item v 3.0 crm.[type].productrow.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the CRM item whose product items are requested

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.productrow.list` method returns the product items of one CRM item. The object type is set by the `<type>` segment of the method name, and the item is set by the `ownerId` condition in the filter.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.productrow.list` ||
|| Lead | `crm.lead.productrow.list` ||
|| Estimate | `crm.quote.productrow.list` ||
|| Invoice | `crm.smartinvoice.productrow.list` ||
|| CRM document | `crm.smartdocument.productrow.list` ||
|| SPA | `crm.smartprocess<entityTypeId>.productrow.list`, for example, `crm.smartprocess1038.productrow.list` ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field. An SPA has product item methods only if linking to catalog products is enabled in the type settings.

The method name is written in lowercase: `crm.smartinvoice.productrow.list`, not `crm.smartInvoice.productRow.list`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../../data-types.md) | Filter in the REST 3.0 format: an array of conditions in the form `["field", "operator", "value"]`. The `ownerId` condition is required, for example, `[["ownerId","=",37]]`. [Parameter description](#filter) ||
|| **select**
[`array`](../../../data-types.md) | List of product item fields to return, for example, `["id","productName","price","quantity"]`. The available fields are described in the [product item methods overview](./index.md#fields).

Default: `id` only ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the format `{"field": "direction"}`. You can sort by the `sort` and `id` fields.

Possible direction values:

- `asc` — ascending
- `desc` — descending

Default: `{"sort": "asc", "id": "asc"}`. If `id` is not in `order`, it is added last in ascending order ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination object. [Parameter description](#pagination) ||
|#

### The filter Parameter {#filter}

The product item filter is simpler than in other REST 3.0 list methods. The method accepts only the conditions from the table and combines them with AND:

#|
|| **Field** | **Operators** | **Description** ||
|| `ownerId` | `=` | CRM item ID. A required condition, only one per filter. The value is a positive integer ||
|| `id` | `=`, `in` | Product item IDs. For `in`, pass an array, for example, `["id","in",[2409,2411]]` ||
|| `productId` | `=`, `in` | Catalog product IDs ||
|#

Filter restrictions:

- each field can be specified only once
- condition groups with `logic`, `negative`, and other operators are not supported, and the method returns the `INVALIDFILTEREXCEPTION` error
- an `in` list contains no more than 1,000 values
- an empty `in` list finds no product items

### The pagination Parameter {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../../data-types.md) | Number of product items per page. Values greater than `1000` are reduced to `1000`. Zero and negative values cause an error.

Default: `50` ||
|| **page**
[`integer`](../../../data-types.md) | Page number, starting from `1`. If `page` is passed, the `offset` parameter is ignored ||
|| **offset**
[`integer`](../../../data-types.md) | Number of product items to skip from the beginning of the list.

Default: `0` ||
|#

{% note info "" %}

The response does not contain the total number of product items. If a page returns fewer product items than specified in `limit`, it is the last page.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.productrow.list`

{% endnote %}

The examples retrieve the product items of an SPA item with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["ownerId","=",37]],"select":["id","productId","productName","price","quantity"],"order":{"sort":"asc"},"pagination":{"limit":50}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.productrow.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["ownerId","=",37]],"select":["id","productId","productName","price","quantity"],"order":{"sort":"asc"},"pagination":{"limit":50},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.productrow.list
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProductRowListResult = {
      items: Array<{
        id: number
        productId: number
        productName: string
        price: number
        quantity: number
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<ProductRowListResult>(
        'crm.smartprocess1038.productrow.list',
        {
          filter: [
            ['ownerId', '=', 37],
          ],
          select: ['id', 'productId', 'productName', 'price', 'quantity'],
          order: {
            sort: 'asc',
          },
          pagination: {
            limit: 50,
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Product rows:', result.items)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listProductRows() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.productrow.list',
            {
              filter: [
                ['ownerId', '=', 37],
              ],
              select: ['id', 'productId', 'productName', 'price', 'quantity'],
              order: {
                sort: 'asc',
              },
              pagination: {
                limit: 50,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Product rows:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listProductRows)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.productrow.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "filter": [
                    ["ownerId", "=", 37],
                ],
                "select": ["id", "productId", "productName", "price", "quantity"],
                "order": {
                    "sort": "asc",
                },
                "pagination": {
                    "limit": 50,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.smartprocess1038.productrow.list',
                [
                    'filter' => [
                        ['ownerId', '=', 37],
                    ],
                    'select' => ['id', 'productId', 'productName', 'price', 'quantity'],
                    'order' => [
                        'sort' => 'asc',
                    ],
                    'pagination' => [
                        'limit' => 50,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'crm.smartprocess1038.productrow.list',
        {
            filter: [
                ['ownerId', '=', 37],
            ],
            select: ['id', 'productId', 'productName', 'price', 'quantity'],
            order: {
                sort: 'asc',
            },
            pagination: {
                limit: 50,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.smartprocess1038.productrow.list',
        [
            'filter' => [
                ['ownerId', '=', 37],
            ],
            'select' => ['id', 'productId', 'productName', 'price', 'quantity'],
            'order' => [
                'sort' => 'asc',
            ],
            'pagination' => [
                'limit' => 50,
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
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.productrow.list", b24.Params{
    	"filter": []any{
    		[]any{"ownerId", "=", 37},
    	},
    	"select": []string{"id", "productId", "productName", "price", "quantity"},
    	"order": b24.Params{
    		"sort": "asc",
    	},
    	"pagination": b24.Params{
    		"limit": 50,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.productrow.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID          int     `json:"id"`
    		ProductID   int     `json:"productId"`
    		ProductName string  `json:"productName"`
    		Price       float64 `json:"price"`
    		Quantity    float64 `json:"quantity"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.ProductName, item.Price, item.Quantity)
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
                "id": 2409,
                "productId": 6461,
                "productName": "Product",
                "price": 100.5,
                "quantity": 2
            },
            {
                "id": 2411,
                "productId": 0,
                "productName": "Installation",
                "price": 4500,
                "quantity": 3
            }
        ]
    },
    "time": {
        "start": 1793709949,
        "finish": 1793709949.853585,
        "duration": 0.8535850048065186,
        "processing": 0,
        "date_start": "2026-11-03T15:45:49+03:00",
        "date_finish": "2026-11-03T15:45:49+03:00",
        "operating_reset_at": 1793710260,
        "operating": 0.3442521095275879
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with response data ||
|| **result.items**
[`object[]`](../../../data-types.md) | Array of product items with the fields from `select`. Without `select`, each product item contains only `id`. If the item has no product items, the array is empty. [Description of product item fields](./index.md#fields) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION",
        "message": "Error validating request object filters.",
        "validation": [
            {
                "message": "Filter for required field \"ownerId\" is required.",
                "field": "filter.ownerId"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Required field `filter` is missing | Pass `filter` with an `ownerId` condition ||
|| `price` | DTO `ProductRowDto` in field `price` requires attribute `Filterable` to perform this request | Filter only by `ownerId`, `id`, and `productId` ||
|| `price` | DTO `ProductRowDto` in field `price` requires attribute `Sortable` to perform this request | Sort only by `sort` and `id` ||
|#

#### Filter Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.ownerId` | Filter for required field `ownerId` is required | Add the condition `["ownerId","=",<item ID>]` to `filter` ||
|| `filter.id` | The list `filter.id` holds more than 1000 elements | Split the list of IDs into parts of up to 1,000 values. The method checks `filter.productId` the same way ||
|#

#### Invalid Filter

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid filter `#FILTER#` | Check the condition against the [filter parameter](#filter) table: only `=` for `ownerId`, `=` or `in` for `id` and `productId`. Remove condition groups with `logic` and repeated fields ||
|#

#### Invalid Pagination

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Incorrect `pagination` parameter value: `#PAGE#` | Pass `limit` greater than zero and `offset` not less than zero ||
|#

#### Item Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | This object type has no item with the `ownerId` ID, or you do not have permission to view it. Check `ownerId` and the method name ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `ProductRowDto` | Check the field names in `select`, `filter`, and `order` against the [description of product item fields](./index.md#fields) ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.deal.productrow.list`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method and make sure linking to products is enabled in the type ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-product-row-get-rest-v3.md)
- [{#T}](./crm-entity-product-row-get-available-for-payment-rest-v3.md)
- [{#T}](./crm-entity-product-row-add-rest-v3.md)
- [{#T}](./crm-entity-product-row-replace-rest-v3.md)
