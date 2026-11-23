# Get a List of SPA Types v 3.0 crm.smartprocess.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user. The method returns only the types the user has access to

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.smartprocess.list` method returns SPA types with filtering, sorting, and pagination.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | List of fields to return for each type. [Parameter description](#select)

If the parameter is not passed, the method returns all fields ||
|| **filter**
[`array`](../../../data-types.md) | Type selection conditions in the REST 3.0 format. [Parameter description](#filter)

If the parameter is not passed, the method returns all available types ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the format `{"field": "direction"}`. [Parameter description](#order)

By default, ascending by `entityTypeId` ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination parameters. [Parameter description](#pagination)

By default, the method returns the first 50 types ||
|#

A type is included in the response if the user is a CRM administrator or an administrator of this SPA, can read items of the type in at least one pipeline, or can manage SPAs. The method skips other types without returning an error.

The method name is written in lowercase: `crm.smartprocess.list`. For the name `crm.smartProcess.list`, the method returns the ["Method Not Found"](#method-not-found) error.

### The select Parameter {#select}

The type fields are described on the [crm.smartprocess.get](./crm-smart-process-get-rest-v3.md#item) method page.

Without `select`, the method returns all fields, including the `relations` links, the `linkedUserFields` bindings, and the `customSections` digital workplaces. If `select` is passed, nested objects are returned only when they are listed in it. The `entityTypeId` field is not added automatically: list it explicitly.

You can select some fields of nested objects with a dot-notation path:

- `relations.parent` — only parent links
- `relations.parent.entityTypeId` — only parent type IDs
- ```linkedUserFields.TASKS_TASK|UF_CRM_TASK``` — only the flag for tasks
- `customSections.title` — only digital workplace names

`select` can be passed as a string with one field: `"select": "entityTypeId"`. For an unknown field, the method returns an [error](#unknown-field).

### The filter Parameter {#filter}

A filter is an array of conditions. The method combines the conditions in the array with AND logic. A condition is written in the format `["field", "operator", value]`, for example, `["isClientEnabled", "=", true]`.

You can filter by all type fields except `description`, `additionalCustomSectionIds`, `relations`, `linkedUserFields`, and `customSections`. Operators:

- `=`, `!=` — for all fields
- `>`, `>=`, `<`, `<=` — for numbers and dates
- `in` — a value from a list, for example, `["entityTypeId", "in", [156, 1038]]`
- `between` — a range of two values, for example, `["entityTypeId", "between", [160, 180]]`

What each operator does is described in the [REST 3.0 overview](../../../rest-v3.md#operators). You cannot search by part of a name: there is no `like` operator.

The value must match the field type: `true` or `false` for `is…` flags, a number for `entityTypeId`, a string for `title` and `code`, and a date in ISO 8601 format for `createdTime` and `updatedTime`, for example, `"2025-01-01T00:00:00+01:00"`. The value `"Y"` for a flag or `null` for `customSectionId` causes a validation error.

To combine conditions with OR logic, pass a group — an object with these keys:

- `type` — the value `filter`
- `logic` — `and` or `or`, `and` by default
- `negative` — `true` selects types that do not meet the group condition
- `conditions` — an array of conditions and nested groups

Example: type `156` or types created after January 1, 2025.

```json
{
    "filter": [
        {
            "type": "filter",
            "logic": "or",
            "conditions": [
                ["entityTypeId", "=", 156],
                ["createdTime", ">", "2025-01-01T00:00:00+01:00"]
            ]
        }
    ]
}
```

{% note warning "" %}

The method does not apply an `in` condition with an empty list, for example, `["entityTypeId", "in", []]`, and returns all types.

{% endnote %}

### The order Parameter {#order}

An object where the key is a field name and the value is the sort direction:

- `ASC` — ascending
- `DESC` — descending

You can sort by the same fields you can filter by. If `order` is not passed or does not contain `entityTypeId`, the method adds ascending sorting by `entityTypeId`.

### The pagination Parameter {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../../data-types.md) | Number of types per page. For values greater than `1000`, the method returns no more than 1000 types. Zero and negative values cause an error.

Default: `50` ||
|| **page**
[`integer`](../../../data-types.md) | Page number, starting from `1`. The offset is calculated as `(page - 1) × limit`.

If `page` is passed, the `offset` parameter is ignored ||
|| **offset**
[`integer`](../../../data-types.md) | Number of types to skip from the beginning of the list. A negative value causes an error.

Default: `0` ||
|#

The response does not contain the total number of types. If a page returns fewer types than specified in `limit`, it is the last page.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess.list`

{% endnote %}

The examples retrieve the first three SPA types with the "Customer" field enabled: the ID, the name, and the pipeline and stage flags.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["entityTypeId","title","isCategoriesEnabled","isStagesEnabled"],"filter":[["isClientEnabled","=",true]],"order":{"entityTypeId":"ASC"},"pagination":{"limit":3}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["entityTypeId","title","isCategoriesEnabled","isStagesEnabled"],"filter":[["isClientEnabled","=",true]],"order":{"entityTypeId":"ASC"},"pagination":{"limit":3},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess.list
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type SmartProcessListResult = {
      items: {
        entityTypeId: number
        title: string
        isCategoriesEnabled: boolean
        isStagesEnabled: boolean
      }[]
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<SmartProcessListResult>(
        'crm.smartprocess.list',
        {
          select: ['entityTypeId', 'title', 'isCategoriesEnabled', 'isStagesEnabled'],
          filter: [
            ['isClientEnabled', '=', true],
          ],
          order: {
            entityTypeId: 'ASC',
          },
          pagination: {
            limit: 3,
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Smart process types:', result.items)
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
      async function listSmartProcessTypes() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess.list',
            {
              select: ['entityTypeId', 'title', 'isCategoriesEnabled', 'isStagesEnabled'],
              filter: [
                ['isClientEnabled', '=', true],
              ],
              order: {
                entityTypeId: 'ASC',
              },
              pagination: {
                limit: 3,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Smart process types:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listSmartProcessTypes)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "select": ["entityTypeId", "title", "isCategoriesEnabled", "isStagesEnabled"],
                "filter": [
                    ["isClientEnabled", "=", True],
                ],
                "order": {"entityTypeId": "ASC"},
                "pagination": {"limit": 3},
            },
            prefer_version=3,
        )
        for item in response["result"]["items"]:
            print(item["entityTypeId"], item["title"])
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
                'crm.smartprocess.list',
                [
                    'select' => ['entityTypeId', 'title', 'isCategoriesEnabled', 'isStagesEnabled'],
                    'filter' => [
                        ['isClientEnabled', '=', true],
                    ],
                    'order' => [
                        'entityTypeId' => 'ASC',
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'crm.smartprocess.list',
        {
            select: ['entityTypeId', 'title', 'isCategoriesEnabled', 'isStagesEnabled'],
            filter: [
                ['isClientEnabled', '=', true],
            ],
            order: {
                entityTypeId: 'ASC',
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.smartprocess.list',
        [
            'select' => ['entityTypeId', 'title', 'isCategoriesEnabled', 'isStagesEnabled'],
            'filter' => [
                ['isClientEnabled', '=', true],
            ],
            'order' => [
                'entityTypeId' => 'ASC',
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
    res, err := client.Core().Call(ctx, "crm.smartprocess.list", b24.Params{
    	"select": []string{"entityTypeId", "title", "isCategoriesEnabled", "isStagesEnabled"},
    	"filter": []any{
    		[]any{"isClientEnabled", "=", true},
    	},
    	"order": b24.Params{
    		"entityTypeId": "ASC",
    	},
    	"pagination": b24.Params{
    		"limit": 3,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess.list: %w", err)
    }

    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("response has no items key")
    }

    var items []struct {
    	EntityTypeID        int    `json:"entityTypeId"`
    	Title               string `json:"title"`
    	IsCategoriesEnabled bool   `json:"isCategoriesEnabled"`
    	IsStagesEnabled     bool   `json:"isStagesEnabled"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.EntityTypeID, it.Title)
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
                "entityTypeId": 156,
                "title": "Purchase",
                "isCategoriesEnabled": true,
                "isStagesEnabled": true
            },
            {
                "entityTypeId": 174,
                "title": "Contract Approval",
                "isCategoriesEnabled": true,
                "isStagesEnabled": true
            },
            {
                "entityTypeId": 177,
                "title": "Equipment Purchase",
                "isCategoriesEnabled": true,
                "isStagesEnabled": true
            }
        ]
    },
    "time": {
        "start": 1795431330,
        "finish": 1795431330.658235,
        "duration": 0.6582350730895996,
        "processing": 0,
        "date_start": "2026-11-23T11:55:30+01:00",
        "date_finish": "2026-11-23T11:55:30+01:00",
        "operating_reset_at": 1795431900,
        "operating": 0
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
[`array`](../../../data-types.md) | Array of SPA types on the current page. If no types match the filter or the page is beyond the end of the list, the array is empty ||
|| **result.items[]**
[`object`](./crm-smart-process-get-rest-v3.md#item) | SPA type. The set of fields is defined by the `select` parameter. [Field description](./crm-smart-process-get-rest-v3.md#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
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
                "message": "This request requires data type \"bool\" in field \"isClientEnabled\".",
                "field": "isClientEnabled"
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
|| `isClientEnabled` | This request requires data type `bool` in field `isClientEnabled` | Pass a value of the field type in the filter condition: `true` or `false`, not `"Y"` ||
|| `customSectionId` | This request requires data type `int` in field `customSectionId` | Pass a number. The filter does not support comparison with `null` ||
|| `description` | DTO `SmartProcessDto` in field `description` requires attribute `Filterable` to perform this request | The field cannot be used in a filter. The fields available for filtering are listed in [The filter Parameter](#filter) ||
|| `description` | DTO `SmartProcessDto` in field `description` requires attribute `Sortable` to perform this request | The field cannot be used for sorting. The fields available for sorting are listed in [The order Parameter](#order) ||
|#

#### Filter Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid filter `Unknown filter condition` | Pass `filter` as an array of conditions, not as a string or a number ||
|| `-` | Invalid filter `Operator ">" cannot be used for dto field "title" with type string.` | For string fields, use `=`, `!=`, or `in` ||
|#

#### Unknown Filter Operator

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown filter operator `like` | Use the operators listed in [The filter Parameter](#filter). There is no `like` operator ||
|#

#### Unknown Field Error {#unknown-field}

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `foo` found in entity `SmartProcessDto` | Check the field name in `select`, `filter`, or `order` against the [type field descriptions](./crm-smart-process-get-rest-v3.md#item) ||
|| `-` | Unknown field `foo` found in entity `SmartProcessRelationsDto` | The dot-notation path contains an unknown nested field. `relations` has only `parent` and `child` ||
|#

#### Sort Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid sort order `UP` | Pass `order` as an object with the `ASC` or `DESC` direction, for example, `{"title":"DESC"}` ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Incorrect `pagination` parameter value: `{"limit":0}` | Pass a positive number in `limit` and a non-negative number in `offset` ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `crm.smartProcess.list` was not found | Write the method name in lowercase: `crm.smartprocess.list` ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-smart-process-get-rest-v3.md)
- [{#T}](./crm-smart-process-add-rest-v3.md)
- [{#T}](./crm-smart-process-field-list-rest-v3.md)
- [{#T}](../items/crm-entity-list-rest-v3.md)
