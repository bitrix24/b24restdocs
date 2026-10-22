# Get List of Catalog Sections v 3.0 catalog.section.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "View catalog products" access permission and permission to read information block sections

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The method `catalog.section.list` returns the sections of a single trade catalog information block page by page.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../data-types.md) | List filter in the REST 3.0 format: an array of conditions of the form `["field", "operator", "value"]`. A condition on `iblockId` is required. [Filter field description](#filter).

The filter syntax is described in the [REST 3.0 overview](../../rest-v3.md#filter) ||
|| **select**
[`array`](../../data-types.md) | Array of strings with the names of the section fields to return in the response, for example `["id","name","iblockSectionId"]`.

Available fields:

- `id` — section identifier
- `iblockId` — information block identifier
- `name` — name
- `iblockSectionId` — parent section identifier
- `active` — activity
- `code` — symbolic code
- `xmlId` — external identifier
- `sort` — sort index
- `description` — description
- `descriptionType` — description format

Default: all fields ||
|| **order**
[`object`](../../data-types.md) | Sorting in the format `{"field": "direction"}`, for example `{"sort":"ASC"}`. Direction: `ASC` — ascending, `DESC` — descending.

Fields available for sorting: `id`, `name`, `active`, `code`, `xmlId`, `sort`.

Default: `{"id":"ASC"}` ||
|| **pagination**
[`object`](../../data-types.md) | Pagination. [Object structure description](#pagination) ||
|#

### Parameter filter {#filter}

The operators `=`, `!=`, `>`, `>=`, `<`, `<=`, `in`, `between` are supported. Conditions are combined with AND logic. Groups with `or` logic and negation are not supported. Two conditions with the same field and operator in one filter cause a validation error.

#|
|| **Field** | **Description** ||
|| **iblockId***
[`integer`](../../data-types.md) | Information block identifier. Only the `=` operator with a single value is supported.

The identifier can be retrieved using the [catalog.catalog.list](../catalog/catalog-catalog-list-rest-v3.md) method ||
|| **iblockSectionId**
[`integer`](../../data-types.md) | Identifier of the parent section. The condition returns only direct subsections, without sections of deeper levels.

The value `0` means the top level. It works with the `=`, `!=`, `>` operators and `in` with a single value: `["iblockSectionId","=",0]` returns top-level sections, `["iblockSectionId",">",0]` returns all nested sections.

The `<` and `<=` operators are not supported ||
|| **id**
[`integer`](../../data-types.md) | Section identifier ||
|| **name**
[`string`](../../data-types.md) | Section name ||
|| **active**
[`string`](../../data-types.md) | Section activity: `Y` or `N` ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the section ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier of the section ||
|#

The `sort`, `description`, and `descriptionType` fields are not available in the filter. An `in` condition with an empty array returns an empty list.

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../data-types.md) | Number of sections per page.

Allowed values: from `1` to `1000`. Values greater than `1000` are reduced to `1000`. Zero and negative values cause an error.

Default: `50` ||
|| **page**
[`integer`](../../data-types.md) | Page number, starting from `1`. Use instead of `offset` ||
|| **offset**
[`integer`](../../data-types.md) | Number of sections to skip. The value must be a multiple of `limit`: for example, with `limit` equal to `50`, the values `0`, `50`, `100` are allowed.

Default: `0` ||
|#

If a page beyond the end of the list is requested, the method returns an empty `items` array. The total number of sections is not returned: keep paging while a page contains `limit` items.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.section.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","iblockSectionId","active","sort"],"filter":[["iblockId","=",25],["iblockSectionId","=",385]],"order":{"sort":"ASC"},"pagination":{"limit":10}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.section.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","iblockSectionId","active","sort"],"filter":[["iblockId","=",25],["iblockSectionId","=",385]],"order":{"sort":"ASC"},"pagination":{"limit":10},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.section.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type SectionListResult = {
      items: Array<{
        id: number
        name: string
        iblockSectionId: number
        active: 'Y' | 'N'
        sort: number
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<SectionListResult>({
        method: 'catalog.section.list',
        params: {
          select: [
            'id',
            'name',
            'iblockSectionId',
            'active',
            'sort',
          ],
          filter: [
            ['iblockId', '=', 25],
            ['iblockSectionId', '=', 385],
          ],
          order: {
            sort: 'ASC',
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
        console.info('Sections:', result.items.length)
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
      async function listSections() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.section.list',
            params: {
              select: [
                'id',
                'name',
                'iblockSectionId',
                'active',
                'sort',
              ],
              filter: [
                ['iblockId', '=', 25],
                ['iblockSectionId', '=', 385],
              ],
              order: {
                sort: 'ASC',
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
          console.info('Sections:', result.items.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listSections)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.section.list(
            select=["id", "name", "iblockSectionId", "active", "sort"],
            filter=[
                ["iblockId", "=", 25],
                ["iblockSectionId", "=", 385],
            ],
            order={
                "sort": "ASC",
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
                'catalog.section.list',
                [
                    'select' => ['id', 'name', 'iblockSectionId', 'active', 'sort'],
                    'filter' => [
                        ['iblockId', '=', 25],
                        ['iblockSectionId', '=', 385],
                    ],
                    'order' => [
                        'sort' => 'ASC',
                    ],
                    'pagination' => [
                        'limit' => 10,
                    ],
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
        'catalog.section.list',
        {
            select: ['id', 'name', 'iblockSectionId', 'active', 'sort'],
            filter: [
                ['iblockId', '=', 25],
                ['iblockSectionId', '=', 385],
            ],
            order: {
                sort: 'ASC',
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
        'catalog.section.list',
        [
            'select' => ['id', 'name', 'iblockSectionId', 'active', 'sort'],
            'filter' => [
                ['iblockId', '=', 25],
                ['iblockSectionId', '=', 385],
            ],
            'order' => [
                'sort' => 'ASC',
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
    res, err := client.Core().Call(ctx, "catalog.section.list", b24.Params{
    	"select": []string{"id", "name", "iblockSectionId", "active", "sort"},
    	"filter": []any{
    		[]any{"iblockId", "=", 25},
    		[]any{"iblockSectionId", "=", 385},
    	},
    	"order": b24.Params{
    		"sort": "ASC",
    	},
    	"pagination": b24.Params{
    		"limit": 10,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.section.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID              int    `json:"id"`
    		Name            string `json:"name"`
    		IblockSectionID int    `json:"iblockSectionId"`
    		Active          string `json:"active"`
    		Sort            int    `json:"sort"`
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
                "id": 387,
                "name": "Bags and Backpacks",
                "iblockSectionId": 385,
                "active": "N",
                "sort": 200
            }
        ]
    },
    "time": {
        "start": 1792665627,
        "finish": 1792665627.622331,
        "duration": 0.622330904006958,
        "processing": 0,
        "date_start": "2026-10-22T13:40:27+02:00",
        "date_finish": "2026-10-22T13:40:27+02:00",
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
[`object`](../../data-types.md) | Object with the list of sections ||
|| **result.items**
[`array`](../../data-types.md) | Array of sections on the page. The set of fields of each section depends on `select`. [Field description](#items) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#items}

The table contains all fields that can be requested in `select`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Section identifier ||
|| **iblockId**
[`integer`](../../data-types.md) | Identifier of the trade catalog information block ||
|| **name**
[`string`](../../data-types.md) | Section name ||
|| **iblockSectionId**
[`integer`](../../data-types.md) | Identifier of the parent section. `0` — a top-level section ||
|| **active**
[`string`](../../data-types.md) | Section activity: `Y` — active, `N` — inactive ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the section. An empty string if the code is not set ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier of the section. An empty string if the identifier is not set ||
|| **sort**
[`integer`](../../data-types.md) | Sort index ||
|| **description**
[`string`](../../data-types.md) | Section description. An empty string if the description is not set ||
|| **descriptionType**
[`string`](../../data-types.md) | Description format: `text` or `html` ||
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
                "message": "Filter for required field \"iblockId\" is required.",
                "field": "filter.iblockId"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Required Filter Error

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.iblockId` | Filter for required field `iblockId` is required | Add the condition `["iblockId","=",25]` to `filter` ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.iblockId` | Filter condition for "iblockId" supports only equality with a single iblock | Pass `iblockId` with the `=` operator and a single value. Retrieve sections of several information blocks with separate requests ||
|| `filter.iblockSectionId` | Filter condition for "iblockSectionId" does not support less than comparisons | Do not use `<` and `<=` for `iblockSectionId` ||
|| `filter.iblockSectionId` | Filter condition for "iblockSectionId" supports the root value 0 only with "=", "!=", ">" or a single value "in" | Do not mix `0` with other identifiers in `in`. Retrieve top-level sections with a separate condition `["iblockSectionId","=",0]` ||
|| `-` | Filter OR groups are not supported | Remove the group with `"logic": "or"` and run several requests ||
|| `-` | Filter NOT groups are not supported | Remove the negation of the condition group ||
|| `-` | Filter has conflicting conditions for the field "#FIELD#" | Keep one condition with this field and operator, for example, replace two `>` conditions with `between` ||
|| `sort` | DTO `SectionDto` in field `sort` requires attribute `Filterable` to perform this request | Remove the field that is not available for filtering from `filter` ||
|| `iblockSectionId` | DTO `SectionDto` in field `iblockSectionId` requires attribute `Sortable` to perform this request | Remove the field that is not available for sorting from `order` ||
|| `-` | pagination.offset must be a non-negative multiple of pagination.limit | Pass an `offset` that is a multiple of `limit`, or use `page` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `SectionDto` | Pass in `select`, `filter`, and `order` only the section fields from the [Items Element](#items) table ||
|#

#### Parameter Format Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `Unknown filter condition` | Pass `filter` as an array of conditions, for example `[["iblockId","=",25]]` ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Unknown filter operator `#OPERATOR#` | Use the operators from the [REST 3.0 overview](../../rest-v3.md#operators) ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `order` | Invalid sort order `#ORDER#` | Pass `order` as an object with the `ASC` or `DESC` direction, for example `{"sort":"ASC"}` ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `#PAGINATION#` | Pass `limit` from `1` to `1000` and a non-negative `offset` ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select`: `#SELECT#` | Pass `select` as an array of strings, for example `["id","name"]` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions for the catalog and the sections of the information block from `iblockId` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-section-add-rest-v3.md)
- [{#T}](./catalog-section-update-rest-v3.md)
- [{#T}](./catalog-section-get-rest-v3.md)
- [{#T}](./catalog-section-delete-rest-v3.md)
- [{#T}](./catalog-section-field-list.md)
