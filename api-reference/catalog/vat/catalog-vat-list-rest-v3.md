# Get List of VAT Rates v 3.0 catalog.vat.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "Manage tax rates" permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.vat.list` method returns a list of VAT rates with filtering, sorting, and pagination.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of rate fields to return in the response.

Available fields:

- `id` — VAT rate identifier
- `timestampX` — date and time of the last change
- `active` — rate activity
- `sort` — sort order
- `name` — rate name
- `rate` — rate value

The `id` field is always returned, even if it is not in `select`. The `["*"]` value is not supported.

Default: all fields ||
|| **filter**
[`array`](../../data-types.md) | A filter in the REST 3.0 format: an array of conditions like `["field", "operator", "value"]`. You can filter by any field from `select`.

For `active`, pass `true` or `false`; the strings `Y` and `N` cause a validation error.

The syntax and operators are described in the [REST 3.0 overview](../../rest-v3.md#filter) ||
|| **order**
[`object`](../../data-types.md) | Sorting in the `{"field": "direction"}` format. You can sort by any field from `select`.

Possible direction values:

- `ASC` — ascending
- `DESC` — descending

Default: `{"id": "ASC"}` ||
|| **pagination**
[`object`](../../data-types.md) | Pagination object. [Object structure](#pagination) ||
|#

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../data-types.md) | The number of rates per page. Values greater than `1000` are reduced to `1000`. Zero, negative, and non-numeric values cause an error.

Default: `50` ||
|| **page**
[`integer`](../../data-types.md) | Page number, starting from `1`. The offset is calculated as `(page - 1) × limit`. Zero and negative values return the first page.

If `page` is passed, the `offset` parameter is ignored ||
|| **offset**
[`integer`](../../data-types.md) | The number of rates to skip from the beginning of the list. A negative value causes an error.

Default: `0` ||
|#

{% note info "" %}

The response does not contain the total number of rates. If a page returns fewer rates than specified in `limit`, it is the last page.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.vat.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","rate","active"],"filter":[["active","=",true]],"order":{"sort":"ASC"},"pagination":{"page":1,"limit":2}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.vat.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","rate","active"],"filter":[["active","=",true]],"order":{"sort":"ASC"},"pagination":{"page":1,"limit":2},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.vat.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type VatListResult = {
      items: Array<{
        id: number
        name?: string
        rate?: number | null
        active?: boolean
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<VatListResult>({
        method: 'catalog.vat.list',
        params: {
          select: ['id', 'name', 'rate', 'active'],
          filter: [
            ['active', '=', true],
          ],
          order: {
            sort: 'ASC',
          },
          pagination: {
            page: 1,
            limit: 2,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('VAT rates:', result.items)
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
      async function listVats() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.vat.list',
            params: {
              select: ['id', 'name', 'rate', 'active'],
              filter: [
                ['active', '=', true],
              ],
              order: {
                sort: 'ASC',
              },
              pagination: {
                page: 1,
                limit: 2,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('VAT rates:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listVats)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.vat.list(
            select=["id", "name", "rate", "active"],
            filter=[
                ["active", "=", True],
            ],
            order={
                "sort": "ASC",
            },
            pagination={
                "page": 1,
                "limit": 2,
            },
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
                'catalog.vat.list',
                [
                    'select' => ['id', 'name', 'rate', 'active'],
                    'filter' => [
                        ['active', '=', true],
                    ],
                    'order' => [
                        'sort' => 'ASC',
                    ],
                    'pagination' => [
                        'page' => 1,
                        'limit' => 2,
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
        'catalog.vat.list',
        {
            select: ['id', 'name', 'rate', 'active'],
            filter: [
                ['active', '=', true],
            ],
            order: {
                sort: 'ASC',
            },
            pagination: {
                page: 1,
                limit: 2,
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
        'catalog.vat.list',
        [
            'select' => ['id', 'name', 'rate', 'active'],
            'filter' => [
                ['active', '=', true],
            ],
            'order' => [
                'sort' => 'ASC',
            ],
            'pagination' => [
                'page' => 1,
                'limit' => 2,
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
    res, err := client.Core().Call(ctx, "catalog.vat.list", b24.Params{
    	"select": []string{"id", "name", "rate", "active"},
    	"filter": []any{
    		[]any{"active", "=", true},
    	},
    	"order": b24.Params{
    		"sort": "ASC",
    	},
    	"pagination": b24.Params{
    		"page":  1,
    		"limit": 2,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.vat.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID     int      `json:"id"`
    		Name   string   `json:"name"`
    		Rate   *float64 `json:"rate"`
    		Active bool     `json:"active"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.Name)
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
                "id": 1,
                "active": true,
                "name": "No VAT",
                "rate": null
            },
            {
                "id": 9,
                "active": true,
                "name": "VAT 19%",
                "rate": 19
            }
        ]
    },
    "time": {
        "start": 1792665623,
        "finish": 1792665623.054244,
        "duration": 0.054244041442871094,
        "processing": 0,
        "date_start": "2026-10-22T13:40:23+02:00",
        "date_finish": "2026-10-22T13:40:23+02:00",
        "operating_reset_at": 1792666200,
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
[`array`](../../data-types.md) | An array of VAT rates on the page. The set of fields depends on `select`. [Field description](#items) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#items}

The table lists all fields that can be requested in `select`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | VAT rate identifier ||
|| **timestampX**
[`datetime`](../../data-types.md) | Date and time of the last rate change ||
|| **active**
[`boolean`](../../data-types.md) | Rate activity: `true` — active, `false` — inactive ||
|| **sort**
[`integer`](../../data-types.md) | Sort order ||
|| **name**
[`string`](../../data-types.md) | VAT rate name ||
|| **rate**
[`double`](../../data-types.md) or `null` | VAT rate value in percent. `null` for the system "No VAT" rate ||
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
                "message": "This request requires data type \"bool\" in field \"active\".",
                "field": "active"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `active` | This request requires data type `bool` in field `active` | Pass `true` or `false` in the filter, not `Y` or `N` ||
|#

#### Filter Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `Unknown filter condition` | Pass `filter` as an array of conditions, for example `[["active","=",true]]` ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Unknown filter operator `#OPERATOR#` | Use the operators from the [REST 3.0 overview](../../rest-v3.md#operators) ||
|#

#### Sorting Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `order` | Invalid sort order `#ORDER#` | Pass `order` as an object, for example `{"sort":"ASC"}`, with the `ASC` or `DESC` direction ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `#PAGINATION#` | Pass a positive number in `limit` and a non-negative number in `offset` ||
|#

#### Unknown Field Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select`, `filter`, `order` | Unknown field `#FIELD#` found in entity `VatDto` | Use only the `id`, `timestampX`, `active`, `sort`, `name`, `rate` fields ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select`: `#SELECT#` | Pass `select` as an array of strings, for example `["id","name"]` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "View product catalog" or "Manage tax rates" permission ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-vat-add-rest-v3.md)
- [{#T}](./catalog-vat-update-rest-v3.md)
- [{#T}](./catalog-vat-get-rest-v3.md)
- [{#T}](./catalog-vat-delete-rest-v3.md)
- [{#T}](./catalog-vat-field-list.md)
