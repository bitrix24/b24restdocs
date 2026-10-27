# Get List of Products in Inventory Documents v 3.0 catalog.document.element.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "View and select warehouses" permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `catalog.document.element.list` method returns a list of product items of inventory documents.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | Array of product item fields to return in the response.

Available fields:

- `id` — product item identifier
- `docId` — document identifier
- `storeFrom` — source warehouse
- `storeTo` — receiving warehouse
- `elementId` — product identifier
- `amount` — quantity
- `purchasingPrice` — purchase price

The `id` field is always returned, even if it is not in `select`. The `["*"]` value is not supported.

Default: all fields ||
|| **filter**
[`array`](../../../data-types.md) | Filter in the REST 3.0 format: an array of conditions of the form `["field", "operator", "value"]`, for example `[["docId", "=", 325]]`. The filter syntax is described in the [REST 3.0 overview](../../../rest-v3.md#filter).

You can filter by all fields from the `select` parameter. Condition groups with `or` logic are supported.

The method returns items only of the documents and warehouses available to the user ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the format `{"field": "direction"}`, for example `{"id": "asc"}`.

Possible direction values:

- `asc` — ascending
- `desc` — descending

You can sort by the `id`, `docId`, `elementId`, `amount`, `purchasingPrice` fields. The `storeFrom` and `storeTo` fields are not available for sorting ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination. [Object description](#pagination) ||
|#

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../../data-types.md) | Number of records per page. Values greater than `1000` are reduced to `1000`, the value `0` causes an error.

Default: `50` ||
|| **page**
[`integer`](../../../data-types.md) | Page number, starting from `1`. If passed, it replaces `offset`. A page beyond the end of the list returns an empty array ||
|| **offset**
[`integer`](../../../data-types.md) | Number of records to skip ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.document.element.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["docId","=",325]],"select":["id","elementId","storeTo","amount","purchasingPrice"],"order":{"id":"asc"},"pagination":{"limit":10}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.element.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["docId","=",325]],"select":["id","elementId","storeTo","amount","purchasingPrice"],"order":{"id":"asc"},"pagination":{"limit":10},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.document.element.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentElementListResult = {
      items: Array<{
        id: number
        elementId?: number
        storeTo?: number
        amount?: number
        purchasingPrice?: number
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<DocumentElementListResult>({
        method: 'catalog.document.element.list',
        params: {
          filter: [
            ['docId', '=', 325],
          ],
          select: ['id', 'elementId', 'storeTo', 'amount', 'purchasingPrice'],
          order: {
            id: 'asc',
          },
          pagination: {
            limit: 10,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Elements:', result.items.length)
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
      async function documentElementList() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.document.element.list',
            params: {
              filter: [
                ['docId', '=', 325],
              ],
              select: ['id', 'elementId', 'storeTo', 'amount', 'purchasingPrice'],
              order: {
                id: 'asc',
              },
              pagination: {
                limit: 10,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Elements:', result.items.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', documentElementList)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.element.list(
            filter=[
                ["docId", "=", 325],
            ],
            select=["id", "elementId", "storeTo", "amount", "purchasingPrice"],
            order={
                "id": "asc",
            },
            pagination={
                "limit": 10,
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
                'catalog.document.element.list',
                [
                    'filter' => [
                        ['docId', '=', 325],
                    ],
                    'select' => ['id', 'elementId', 'storeTo', 'amount', 'purchasingPrice'],
                    'order' => [
                        'id' => 'asc',
                    ],
                    'pagination' => [
                        'limit' => 10,
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
        'catalog.document.element.list',
        {
            filter: [
                ['docId', '=', 325],
            ],
            select: ['id', 'elementId', 'storeTo', 'amount', 'purchasingPrice'],
            order: {
                id: 'asc',
            },
            pagination: {
                limit: 10,
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
        'catalog.document.element.list',
        [
            'filter' => [
                ['docId', '=', 325],
            ],
            'select' => ['id', 'elementId', 'storeTo', 'amount', 'purchasingPrice'],
            'order' => [
                'id' => 'asc',
            ],
            'pagination' => [
                'limit' => 10,
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
    res, err := client.Core().Call(ctx, "catalog.document.element.list", b24.Params{
    	"filter":     []any{
    		[]any{"docId", "=", 325},
    	},
    	"select":     []string{"id", "elementId", "storeTo", "amount", "purchasingPrice"},
    	"order":      b24.Params{
    		"id": "asc",
    	},
    	"pagination": b24.Params{
    		"limit": 10,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.document.element.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID        int     `json:"id"`
    		ElementID int     `json:"elementId"`
    		Amount    float64 `json:"amount"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.ElementID, item.Amount)
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
                "id": 59,
                "storeTo": 17,
                "elementId": 7573,
                "amount": 5,
                "purchasingPrice": 120
            },
            {
                "id": 65,
                "storeTo": 17,
                "elementId": 7571,
                "amount": 10,
                "purchasingPrice": 450
            }
        ]
    },
    "time": {
        "start": 1792665368,
        "finish": 1792665368.395274,
        "duration": 0.39527392387390137,
        "processing": 0,
        "date_start": "2026-10-22T13:36:08+02:00",
        "date_finish": "2026-10-22T13:36:08+02:00",
        "operating_reset_at": 1792665960,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Response data object ||
|| **result.items**
[`array`](../../../data-types.md) | Array of product items. The set of fields depends on `select`. [Field description](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the document product item ||
|| **docId**
[`integer`](../../../data-types.md) | Identifier of the inventory document ||
|| **storeFrom**
[`integer`](../../../data-types.md) | Identifier of the source warehouse. `0` — no warehouse specified ||
|| **storeTo**
[`integer`](../../../data-types.md) | Identifier of the receiving warehouse. `0` — no warehouse specified ||
|| **elementId**
[`integer`](../../../data-types.md) | Product identifier ||
|| **amount**
[`double`](../../../data-types.md) | Product quantity ||
|| **purchasingPrice**
[`double`](../../../data-types.md) | Purchase price in the document currency ||
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
                "message": "DTO \"DocumentElementDto\" in field \"storeTo\" requires attribute \"Sortable\" to perform this request.",
                "field": "storeTo"
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
|| `storeFrom`, `storeTo` | DTO `DocumentElementDto` in field `#FIELD#` requires attribute `Sortable` to perform this request | Remove `storeFrom` and `storeTo` from `order` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select`, `filter`, `order` | Unknown field `#FIELD#` found in entity `DocumentElementDto` | Specify only the fields from the `select` parameter description ||
|#

#### Filter Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `Unknown filter condition` | Pass `filter` as an array of conditions, for example `[["docId","=",325]]` ||
|#

#### Sorting Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `order` | Invalid sort order `#ORDER#` | Pass the `asc` or `desc` direction ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `{"limit":0}` | Pass a number from `1` to `1000` in `limit` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "View product catalog" or "View and select warehouses" permission ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-element-add-rest-v3.md)
- [{#T}](./catalog-document-element-update-rest-v3.md)
- [{#T}](./catalog-document-element-delete-rest-v3.md)
- [{#T}](./catalog-document-element-field-list.md)
- [{#T}](./catalog-document-element-field-get.md)
- [{#T}](../catalog-document-list-rest-v3.md)
