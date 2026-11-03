# Get a List of CRM Items v 3.0 crm.[type].list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view items of this CRM object type. For my company, a CRM administrator

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.list` method returns a list of CRM items with filtering, sorting, and pagination. The object type is set by the `<type>` segment of the method name.

The list includes only the items the user can view. The method skips items the user has no access to without returning an error.

## Method Name for the Object Type

#|
|| **Object type** | **Method** | **Fields** ||
|| Deal | `crm.deal.list` | [Deal fields](./fields-deal.md) ||
|| Lead | `crm.lead.list` | [Lead fields](./fields-lead.md) ||
|| Contact | `crm.contact.list` | [Contact fields](./fields-contact.md) ||
|| Company | `crm.company.list` | [Company fields](./fields-company.md) ||
|| Estimate | `crm.quote.list` | [Estimate fields](./fields-quote.md) ||
|| Invoice | `crm.smartinvoice.list` | [Invoice fields](./fields-smart-invoice.md) ||
|| My company | `crm.mycompany.list` | [My company fields](./fields-my-company.md) ||
|| SPA | `crm.smartprocess<entityTypeId>.list`, for example, `crm.smartprocess1038.list` | [SPA fields](./fields-smart-process.md) ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field.

The method name is written in lowercase: `crm.smartinvoice.list`, not `crm.smartInvoice.list`. If the name is written in a different case or contains a nonexistent `entityTypeId`, the method returns the ["Method Not Found"](#method-not-found) error.

The number of items matching the same filter, without retrieving the list, is returned by the [crm.&lt;type&gt;.aggregate](./crm-entity-aggregate-rest-v3.md) method.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | List of fields to return for each item. [Parameter description](#select)

If the parameter is not passed, each item in the list contains only `id` ||
|| **filter**
[`array`](../../../data-types.md) | Item selection conditions in the REST 3.0 format. [Parameter description](#filter)

If the parameter is not passed, the method returns all available items ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the format `{"field": "direction"}`. [Parameter description](#order)

Default: `{"id": "ASC"}` ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination parameters. [Parameter description](#pagination)

By default, the method returns the first 50 items ||
|#

### The select Parameter {#select}

Field names are written in camelCase: `title`, `stageId`, `assignedById`. Each object type has its own set of fields, which are described on these pages:

- [deal fields](./fields-deal.md)
- [lead fields](./fields-lead.md)
- [contact fields](./fields-contact.md)
- [company fields](./fields-company.md)
- [estimate fields](./fields-quote.md)
- [invoice fields](./fields-smart-invoice.md)
- [my company fields](./fields-my-company.md)
- [SPA fields](./fields-smart-process.md)

The list of fields is returned by the [crm.&lt;type&gt;.field.list](../fields/crm-entity-field-list-rest-v3.md) method.

Items in the response contain only the fields from `select`. The `id` field is not added automatically: include it in the list if you need it in the response. An empty array `[]` returns items as empty arrays. The asterisk `*` is not supported.

Related objects are requested by the field name without the `Id` suffix: `assignedBy`, `stage`, `category`, `company`. There are two ways:

- the whole related object name, for example, `assignedBy`, returns all fields of the object
- a dot-notation path, for example, `assignedBy.firstName`, `stage.name`, or `company.title`, returns only the selected fields

A path can contain only one dot: `company.assignedBy.firstName` causes an unknown field error. For employees — `assignedBy`, `createdBy`, and others — the name is stored in the `firstName` and `lastName` fields; there is no `name` field. For more information about related fields, see the [REST 3.0 overview](../../../rest-v3.md#connection).

Custom fields are passed by name, for example, `UF_CRM_1687529546`. Their list is returned by the [crm.&lt;type&gt;.field.custom.list](../fields/crm-entity-field-custom-list-rest-v3.md) method.

An unknown field in `select` — of the item itself or of a related object — causes the `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION` error. The method does not silently skip such a field.

### The filter Parameter {#filter}

A filter is an array of conditions. The conditions in the array are combined with AND logic. A condition is written in one of these ways:

- `["field", "operator", value]` — for example, `["opportunity", ">=", 10000]`
- `["field", value]` — the `=` operator, or `in` for an array value: `["id", [17, 19]]`

Operators: `=`, `!=`, `>`, `>=`, `<`, `<=`, `in`, `between`. They are described in the [REST 3.0 overview](../../../rest-v3.md#operators). There are no other operators: `like` and partial string search are not supported, and `["title", "=", "Supply"]` finds only items with exactly this title.

You can filter by fields whose `filterable` attribute is `true` in the [crm.&lt;type&gt;.field.list](../fields/crm-entity-field-list-rest-v3.md) response. The value must match the field type:

#|
|| **Field type** | **Value format** ||
|| `int`, `float` | Number: `69`, `10000` ||
|| `bool` | `true` or `false`. The strings `Y` and `N` cause a validation error ||
|| `date-time` | Date and time with a time zone: `2026-11-03T14:41:00+03:00`. A value without a time zone or a date only causes a validation error ||
|| `date` | Date: `2026-11-01` ||
|| `string` | String, compared for an exact match ||
|#

#### Condition Groups

To combine conditions with OR logic, pass a group — an object with these keys:

- `type` — the value `filter`. Inside the `filter` array, the key is required: a group without it causes the `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION` error
- `logic` — `and` or `or`, `and` by default
- `negative` — `true` inverts the group: items that do not meet the group condition match
- `conditions` — an array of conditions and nested groups

Example: items of pipeline `69` with an amount of 10,000 or more whose stage is `DT1038_69:CLIENT` or whose responsible person is user `1`.

```json
{
    "filter": [
        ["categoryId", "=", 69],
        ["opportunity", ">=", 10000],
        {
            "type": "filter",
            "logic": "or",
            "conditions": [
                ["stageId", "=", "DT1038_69:CLIENT"],
                ["assignedById", "=", 1]
            ]
        }
    ]
}
```

If the whole filter is a single group, you can pass it as an object instead of an array: `"filter": {"logic": "or", "conditions": [...]}`. In this case, `type` can be omitted.

Filter limits:

- group nesting — up to 8 levels
- conditions and groups in a filter — up to 100
- values in all `in` lists of a filter — up to 1000

#### Related Fields in the Filter

A condition can be set on a field of a related object using dot notation:

- a field of a linked CRM item — `["company.title", "=", "Müller GmbH"]`, `["myCompany.title", "=", "My company"]`, `["contact.name", "=", "Klaus"]`
- a phone, email, website, or messenger value — `["phone.value", "=", "+49300000000"]`, for a deal — `["contact.phone.value", "=", "+49300000000"]`. Only the `=` and `in` operators are available for these fields
- a UTM tag — `["utm.source", "=", "google"]`, the fields `source`, `medium`, `campaign`, `content`, `term`
- the time of the last communication — `["lastCommunication.callTime", ">", "2026-11-01T00:00:00+03:00"]`

Fields of employees, stages, pipelines, and other reference lists cannot be filtered using dot notation: `["assignedBy.lastName", "=", "Schmidt"]` causes the `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION` error. Filter by ID instead: `assignedById`, `stageId`, `categoryId`.

#### Empty Values and Multiple Fields

The `null` value can be passed only with the `=` and `!=` operators and only for fields that can be empty, for example, `companyId`, `contactId`, `myCompanyId`. For other fields, `null` causes a type validation error.

An empty link to a CRM item is stored in different ways: in the response, `companyId` is `null`, but the database may contain `0`. To find all items without a company, check both values:

```json
{
    "filter": [
        {
            "type": "filter",
            "logic": "or",
            "conditions": [
                ["companyId", "=", 0],
                ["companyId", "=", null]
            ]
        }
    ]
}
```

For the multiple fields `contactsId` and `observersId`, only the `=`, `!=`, and `in` operators are available. The condition `["contactsId", "in", [5, 7]]` finds items linked to at least one of the contacts. For ID fields of linked items, for example, `contactId`, the `between` operator is not supported.

{% note warning "" %}

If `filter` is passed as neither an array nor an object, for example, as a string or a number, the method does not return an error: the condition is not applied, and the response contains all available items. An object with fields such as `{"id": 17}` causes the `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION` error.

{% endnote %}

### The order Parameter {#order}

An object where the key is a field name and the value is the sort direction. You can sort by fields whose `sortable` attribute is `true` in the [crm.&lt;type&gt;.field.list](../fields/crm-entity-field-list-rest-v3.md) response. Dot-notation fields of related objects and multiple fields, for example, `contactsId`, do not support sorting.

Possible direction values:

- `ASC` — ascending
- `DESC` — descending

The direction is case-insensitive: `asc` and `ASC` work the same way.

If several fields are passed, items are sorted by them in the listed order: `{"stageId": "ASC", "opportunity": "DESC"}`. If `id` is not in `order`, the method adds it as the last key with the `ASC` direction. As a result, the order of items with equal values does not change between pages.

String fields are sorted by value. For example, `stageId` is sorted by the stage code, not by the stage position in the pipeline.

Default: `{"id": "ASC"}`

### The pagination Parameter {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../../data-types.md) | Number of items per page. Values greater than `1000` are reduced to `1000`. Zero, negative, and non-numeric values cause an error.

Default: `50` ||
|| **page**
[`integer`](../../../data-types.md) | Page number, starting from `1`. The offset is calculated as `(page - 1) × limit`. Zero and negative values return the first page.

If `page` is passed, the `offset` parameter is ignored ||
|| **offset**
[`integer`](../../../data-types.md) | Number of items to skip from the beginning of the list. A negative value causes an error.

Default: `0` ||
|#

Other keys in `pagination` cause an error. The method ignores the `start` parameter from the previous REST version.

{% note info "" %}

The response does not contain the total number of items. If a page returns fewer items than specified in `limit`, it is the last page. The number of items matching a filter is returned by the [crm.&lt;type&gt;.aggregate](./crm-entity-aggregate-rest-v3.md) method.

{% endnote %}

## Differences Between Object Types

- `crm.company.list` does not return companies from the "My companies" list. Retrieve them with the `crm.mycompany.list` method
- `crm.deal.list` does not return recurring deal templates. The method also skips templates of recurring invoices and of SPAs with recurrence enabled
- for an SPA, the set of fields depends on the type settings: for example, without stages there is no `stageId`, and without client linking there are no `companyId` and `contactId`. For details, see the [SPA fields](./fields-smart-process.md) page

{% note warning "" %}

For SPAs, the `stageSemanticId` field and the `stageSemantic` object are present in the `crm.<type>.field.list` response, but in `select`, `filter`, and `order` they cause the error `Unknown field stageSemanticId`. Retrieve the stage group of an SPA from the `stage` object — the `semanticId` field. For deals, `stageSemanticId` works.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.list`

{% endnote %}

The examples retrieve SPA items with the type ID `1038` from pipeline `69` with an amount of 10,000 or more — the first page of two items, sorted by amount in descending order. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","title","stageId","opportunity","assignedBy.firstName"],"filter":[["categoryId","=",69],["opportunity",">=",10000]],"order":{"opportunity":"DESC"},"pagination":{"limit":2,"page":1}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","title","stageId","opportunity","assignedBy.firstName"],"filter":[["categoryId","=",69],["opportunity",">=",10000]],"order":{"opportunity":"DESC"},"pagination":{"limit":2,"page":1},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.list
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ItemListResult = {
      items: {
        id: number
        title: string
        stageId: string
        opportunity: number
        assignedBy: {
          firstName: string
        }
      }[]
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<ItemListResult>(
        'crm.smartprocess1038.list',
        {
          select: ['id', 'title', 'stageId', 'opportunity', 'assignedBy.firstName'],
          filter: [
            ['categoryId', '=', 69],
            ['opportunity', '>=', 10000],
          ],
          order: {
            opportunity: 'DESC',
          },
          pagination: {
            limit: 2,
            page: 1,
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Items:', result.items)
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
      async function listItems() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.list',
            {
              select: ['id', 'title', 'stageId', 'opportunity', 'assignedBy.firstName'],
              filter: [
                ['categoryId', '=', 69],
                ['opportunity', '>=', 10000],
              ],
              order: {
                opportunity: 'DESC',
              },
              pagination: {
                limit: 2,
                page: 1,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Items:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listItems)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "select": ["id", "title", "stageId", "opportunity", "assignedBy.firstName"],
                "filter": [
                    ["categoryId", "=", 69],
                    ["opportunity", ">=", 10000],
                ],
                "order": {"opportunity": "DESC"},
                "pagination": {"limit": 2, "page": 1},
            },
            prefer_version=3,
        )
        for item in response["result"]["items"]:
            print(item["id"], item["title"], item["opportunity"])
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
                'crm.smartprocess1038.list',
                [
                    'select' => ['id', 'title', 'stageId', 'opportunity', 'assignedBy.firstName'],
                    'filter' => [
                        ['categoryId', '=', 69],
                        ['opportunity', '>=', 10000],
                    ],
                    'order' => [
                        'opportunity' => 'DESC',
                    ],
                    'pagination' => [
                        'limit' => 2,
                        'page' => 1,
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
        'crm.smartprocess1038.list',
        {
            select: ['id', 'title', 'stageId', 'opportunity', 'assignedBy.firstName'],
            filter: [
                ['categoryId', '=', 69],
                ['opportunity', '>=', 10000],
            ],
            order: {
                opportunity: 'DESC',
            },
            pagination: {
                limit: 2,
                page: 1,
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
        'crm.smartprocess1038.list',
        [
            'select' => ['id', 'title', 'stageId', 'opportunity', 'assignedBy.firstName'],
            'filter' => [
                ['categoryId', '=', 69],
                ['opportunity', '>=', 10000],
            ],
            'order' => [
                'opportunity' => 'DESC',
            ],
            'pagination' => [
                'limit' => 2,
                'page' => 1,
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
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.list", b24.Params{
    	"select": []string{"id", "title", "stageId", "opportunity", "assignedBy.firstName"},
    	"filter": []any{
    		[]any{"categoryId", "=", 69},
    		[]any{"opportunity", ">=", 10000},
    	},
    	"order": b24.Params{
    		"opportunity": "DESC",
    	},
    	"pagination": b24.Params{
    		"limit": 2,
    		"page":  1,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.list: %w", err)
    }

    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("response has no items key")
    }

    var items []struct {
    	ID          int     `json:"id"`
    	Title       string  `json:"title"`
    	StageID     string  `json:"stageId"`
    	Opportunity float64 `json:"opportunity"`
    	AssignedBy  struct {
    		FirstName string `json:"firstName"`
    	} `json:"assignedBy"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.Title, it.Opportunity)
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
                "id": 21,
                "title": "Office equipment repair",
                "stageId": "DT1038_69:CLIENT",
                "opportunity": 95000,
                "assignedBy": {
                    "firstName": "Anna"
                }
            },
            {
                "id": 19,
                "title": "Cartridge supply",
                "stageId": "DT1038_69:PREPARATION",
                "opportunity": 48000,
                "assignedBy": {
                    "firstName": "Anna"
                }
            }
        ]
    },
    "time": {
        "start": 1793706441,
        "finish": 1793706441.814752,
        "duration": 0.8147521018981934,
        "processing": 0,
        "date_start": "2026-11-03T14:47:21+03:00",
        "date_finish": "2026-11-03T14:47:21+03:00",
        "operating_reset_at": 1793706780,
        "operating": 0.14652299880981445
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
[`array`](../../../data-types.md) | Array of items on the current page. If no items match the filter or the page is beyond the end of the list, the array is empty ||
|| **result.items[]**
[`object`](../../../data-types.md) | CRM item. The set of fields is defined by the `select` parameter; without it, the item contains only `id`. Related fields from `select` are returned as nested objects, for example, `"assignedBy": {"firstName": "Anna"}`. The fields are described on the fields page of the object type ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION",
        "message": "Invalid filter \"{\"logic\":\"or\",\"conditions\":[[\"id\",17],[\"id\",\"in\",[21,23]]]}\"."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `#FIELD#` | DTO `#DTO#` in field `#FIELD#` requires attribute `Filterable` to perform this request | The field cannot be used in a filter. Use fields whose `filterable` attribute is `true` in `crm.<type>.field.list` ||
|| `#FIELD#` | DTO `#DTO#` in field `#FIELD#` requires attribute `Sortable` to perform this request | The field cannot be used for sorting. Use fields whose `sortable` attribute is `true` in `crm.<type>.field.list` ||
|| `#FIELD#` | This request requires data type `#TYPE#` in field `#FIELD#` | Pass a value of the field type: a number, `true` or `false`, or a date and time with a time zone. The error also occurs when `null` is passed for a field that cannot be empty ||
|#

#### Filter Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid filter `#FILTER#` | Check the condition from the error message. Common causes: a group inside an array without `"type": "filter"`, an invalid `logic` value, a condition that does not consist of two or three elements, an object such as `{"id": 17}` instead of an array of conditions, an operator not allowed for the field, `null` with an operator other than `=` and `!=`, a dot-notation path to an employee or reference list field ||
|| `-` | Invalid filter `Filter node limit exceeded.` | Reduce the filter to 100 conditions and groups ||
|| `-` | Invalid filter `Filter depth limit exceeded.` | Reduce group nesting to 8 levels ||
|| `-` | Invalid filter `Filter collection operand limit exceeded.` | Pass no more than 1000 values in all `in` lists ||
|#

#### Unknown Filter Operator

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown filter operator `#OPERATOR#` | Use the operators from the [REST 3.0 overview](../../../rest-v3.md#operators) ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `#DTO#` | Check the field name in `select`, `filter`, or `order` on the fields page of the object type. The error also occurs for a dot-notation path in `order` and a path with two dots in `select` ||
|#

#### Select Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid field in `select`: #SELECT# | Pass `select` as an array of strings, for example, `["id","title"]` ||
|#

#### Sort Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid sort order `#ORDER#` | Pass `order` as an object, for example, `{"opportunity":"DESC"}`, with the `ASC` or `DESC` direction ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Incorrect `pagination` parameter value: #PAGINATION# | Pass `pagination` as an object with only the `limit`, `page`, and `offset` keys. Pass a positive number in `limit`, a non-negative number in `offset`, and a number in `page` ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.smartinvoice.list`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-add-rest-v3.md)
- [{#T}](./crm-entity-update-rest-v3.md)
- [{#T}](./crm-entity-get-rest-v3.md)
- [{#T}](./crm-entity-delete-rest-v3.md)
- [{#T}](./crm-entity-aggregate-rest-v3.md)
- [{#T}](../fields/crm-entity-field-list-rest-v3.md)
