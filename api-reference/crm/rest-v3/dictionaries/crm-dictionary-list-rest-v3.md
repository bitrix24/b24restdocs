# Get a List of CRM Directory Values v 3.0 crm.dictionary.[directory].list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view leads, deals, or other CRM objects

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.dictionary.<directory>.list` method returns CRM directory values, for example, all deal types or all sources. The directory is set by the `<directory>` segment of the method name.

To specify a deal type, pass the value ID in the `typeId` field: for the "Sales" type, it is `SALE`. The IDs of all deal types are returned by the `crm.dictionary.dealtype.list` method.

## Method Name for the Directory

#|
|| **Directory** | **Method** | **Where the value is used** ||
|| Deal types | `crm.dictionary.dealtype.list` | The `typeId` field of a deal ||
|| Contact types | `crm.dictionary.contacttype.list` | The `typeId` field of a contact ||
|| Company types | `crm.dictionary.companytype.list` | The `typeId` field of a company ||
|| Salutations | `crm.dictionary.honorific.list` | The `honorificId` field of a contact and a lead ||
|| Number of employees | `crm.dictionary.employees.list` | The `employeesId` field of a company ||
|| Industries | `crm.dictionary.industry.list` | The `industryId` field of a company ||
|| Sources | `crm.dictionary.source.list` | The `sourceId` field of a deal, lead, contact, and invoice ||
|| Phone types | `crm.dictionary.phonetype.list` | The `valueTypeId` key in the `phone` field ||
|| Email types | `crm.dictionary.emailtype.list` | The `valueTypeId` key in the `email` field ||
|| Website types | `crm.dictionary.webtype.list` | The `valueTypeId` key in the `web` field ||
|| Messenger types | `crm.dictionary.imtype.list` | The `valueTypeId` key in the `im` field ||
|| Address types | `crm.dictionary.addresstype.list` | The `typeId` field of an address ||
|| CRM object types | `crm.dictionary.entitytype.list` | The `entityTypeId` field ||
|| Stage groups | `crm.dictionary.stagesemantic.list` | The `stageSemanticId` field ||
|| Activity providers | `crm.dictionary.activityprovider.list` | The `providerId` field of an activity ||
|#

The method name is written in lowercase: `crm.dictionary.dealtype.list`, not `crm.dictionary.dealType.list`. If the name is written in a different case or contains an unknown directory, the method returns the ["Method Not Found"](#method-not-found) error.

A single value by ID is returned by the [crm.dictionary.&lt;directory&gt;.get](./crm-dictionary-get-rest-v3.md) method.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | List of fields to return for each value. [Parameter description](#select)

If the parameter is not passed, the method returns all fields ||
|| **filter**
[`array`](../../../data-types.md) | Value selection conditions in the REST 3.0 format. [Parameter description](#filter)

If the parameter is not passed, the method returns all values ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the format `{"field": "direction"}`. [Parameter description](#order) ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination parameters. [Parameter description](#pagination)

If the parameter is not passed, CRM value directories return the first 50 values, and the other directories return all values ||
|#

### The select Parameter {#select}

Each directory has its own set of fields, which is described in the [Directory Fields](#fields) section.

Values in the response contain only the fields from `select`. An empty array `[]` returns all fields. For an unknown field, the method returns an error.

If you only need the names of the provider activity types, pass the `types.name` path in `select`, and the other keys are left out of the response. Dot notation works only for the `types` field of the `activityprovider` directory.

### The filter Parameter {#filter}

A filter is an array of conditions. The method combines the conditions in the array with AND logic. A condition is written in one of these ways:

- `["field", "operator", value]` — for example, `["sort", ">=", 20]`
- `["field", value]` — without an operator: the method uses `=`, or `in` for an array value, for example, `["id", ["CALL", "WEB"]]`

The operators available for each directory are listed in the [Directory Fields](#fields) section, and what each operator does is described in the [REST 3.0 overview](../../../rest-v3.md#operators). Partial string search is not supported: there is no `like` operator, and `["name", "=", "Sales"]` finds only the value with exactly this name.

The value must match the field type: a number for `integer`, `true` or `false` for `boolean`, and a string for `string`. The string `"10"` for the `sort` field or `"N"` for the `isSystem` field causes a validation error.

To combine conditions with OR logic, pass a group — an object with these keys:

- `type` — the value `filter`
- `logic` — `and` or `or`, `and` by default
- `negative` — `true` selects the values for which the group condition is not met. Works only in CRM value directories and in the `entitytype` directory
- `conditions` — an array of conditions and nested groups

Example: the `SALE` deal type or types with a sort order greater than 40.

```json
{
    "filter": [
        {
            "type": "filter",
            "logic": "or",
            "conditions": [
                ["id", "=", "SALE"],
                ["sort", ">", 40]
            ]
        }
    ]
}
```

If `filter` is passed as a string, a number, or an object such as `{"id": "SALE"}`, the method returns the `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION` error.

### The order Parameter {#order}

An object where the key is a field name and the value is the sort direction:

- `ASC` — ascending
- `DESC` — descending

The direction is case-insensitive. The fields available for sorting are listed in the [Directory Fields](#fields) section.

If `order` is not passed, the order depends on the directory:

- CRM value directories — by `sort` in ascending order
- stage groups — by `id`
- other directories — in the order in which Bitrix24 stores them

### The pagination Parameter {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../../data-types.md) | Number of values per page. For a value greater than `1000`, the method returns no more than 1000 values. Zero, negative, and non-numeric values cause an error.

Default: `50` ||
|| **page**
[`integer`](../../../data-types.md) | Page number, starting from `1`. The offset is calculated as `(page - 1) × limit`. Zero and negative values return the first page.

If `page` is passed, the `offset` parameter is ignored ||
|| **offset**
[`integer`](../../../data-types.md) | Number of values to skip from the beginning of the list. A negative value causes an error.

Default: `0` ||
|#

{% note warning "" %}

The method does not check unknown keys in `pagination`. If you pass `{"size": 10}` instead of `{"limit": 10}`, there is no error, and the response returns up to 50 values.

{% endnote %}

The response does not contain the total number of values. If a page returns fewer values than specified in `limit`, it is the last page.

## Directory Fields {#fields}

Directories differ in their set of fields, filter operators, and sort fields. Field descriptions with the `filterable` and `sortable` attributes are returned by the [crm.dictionary.&lt;directory&gt;.field.list](./index.md#field-methods) method.

### CRM Value Directories {#crm-values}

The `dealtype`, `contacttype`, `companytype`, `honorific`, `employees`, `industry`, and `source` directories. Values of these directories can be created, updated, and deleted with the [crm.dictionary.&lt;directory&gt;.add](./crm-dictionary-add-rest-v3.md), [crm.dictionary.&lt;directory&gt;.update](./crm-dictionary-update-rest-v3.md), and [crm.dictionary.&lt;directory&gt;.delete](./crm-dictionary-delete-rest-v3.md) methods.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Value ID, for example, `SALE` or `EMPLOYEES_2`. The app passes this value in a CRM item field, for example, in the `typeId` field of a deal ||
|| **name**
[`string`](../../../data-types.md) | Value name ||
|| **sort**
[`integer`](../../../data-types.md) | Sort order ||
|| **isSystem**
[`boolean`](../../../data-types.md) | Indicates a default value. Possible values:

- `true` — a default value, which cannot be deleted
- `false` — the value can be deleted ||
|#

You can filter by all fields with the operators `=`, `!=`, `>`, `>=`, `<`, `<=`, `in`. The `between` operator works only for `sort`. You can sort by `id`, `name`, `sort`.

### Communication Types {#communication-types}

The `phonetype`, `emailtype`, `webtype`, and `imtype` directories — types of phone numbers, email addresses, websites, and messengers. The app passes the type ID in the `valueTypeId` key of a multiple field value, for example, `{"valueTypeId": "WORK", "value": "+49300000000"}`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Type ID, for example, `WORK` or `MOBILE` ||
|| **name**
[`string`](../../../data-types.md) | Full name, for example, "Work Phone". Can be `null` ||
|| **nameShort**
[`string`](../../../data-types.md) | Short name, for example, "Work Phone". Can be `null` ||
|| **isDeprecated**
[`boolean`](../../../data-types.md) | Indicates a deprecated type. Possible values:

- `true` — the type is deprecated, for example, `SKYPE` or `ICQ`
- `false` — the type is in use ||
|#

You can filter by all fields with the operators `=`, `!=`, `in`. You can sort by all fields.

### Address Types {#address-types}

The `addresstype` directory — types of contact and company addresses, for example, a legal or street address.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Address type ID, for example, `6` ||
|| **name**
[`string`](../../../data-types.md) | Type code, for example, `REGISTERED` ||
|| **title**
[`string`](../../../data-types.md) | Type name, for example, "Legal address" ||
|#

You can filter by all fields with the operators `=`, `!=`, `in`. You can sort by all fields.

### Stage Groups {#stage-semantics}

The `stagesemantic` directory — the groups to which a CRM item stage belongs.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Group ID. Possible values:

- `P` — in progress
- `S` — successful
- `F` — unsuccessful ||
|| **name**
[`string`](../../../data-types.md) | Group name ||
|#

You can filter by all fields with the operators `=`, `!=`, `in`. You can sort by all fields.

### CRM Object Types {#entity-types}

The `entitytype` directory contains only system object types: lead, deal, contact, company, estimate, invoice, document, and company document. It contains no SPAs. Their `entityTypeId` IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Object type ID `entityTypeId`, for example, `2` for a deal ||
|| **code**
[`string`](../../../data-types.md) | Type code, for example, `Deal` or `SmartInvoice` ||
|| **title**
[`string`](../../../data-types.md) | Type name ||
|#

You can filter by all fields with the operators `=`, `!=`, `>`, `>=`, `<`, `<=`, `in`. The `between` operator works only for `id`. You can sort by all fields.

### Activity Providers {#activity-providers}

The `activityprovider` directory — CRM activity providers, for example, universal activity, meeting, call, email, task.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Provider ID, for example, `CRM_TODO` or `VOXIMPLANT_CALL` ||
|| **name**
[`string`](../../../data-types.md) | Provider name ||
|| **isAvailable**
[`boolean`](../../../data-types.md) | Indicates whether the provider is available in Bitrix24. Possible values:

- `true` — available
- `false` — not available, for example, the required feature is not connected ||
|| **isActive**
[`boolean`](../../../data-types.md) | Indicates whether the provider is enabled. Possible values:

- `true` — enabled
- `false` — disabled. For example, the external channel provider `CRM_EXTERNAL_CHANNEL` is disabled until an external channel is connected to Bitrix24 ||
|| **types**
[`object[]`](../../../data-types.md) | Activity types of the provider. Each type has the ID `providerTypeId`, the name `name`, and the `directions` object with direction names, for example, `{"incoming": "Inbound call", "outgoing": "Outbound call"}` ||
|| **defaultTypeId**
[`string`](../../../data-types.md) | Default activity type, for example, `CALL`. Can be `null` ||
|| **capabilities**
[`string[]`](../../../data-types.md) | Capabilities of the provider activities, for example, `calendar`, `completion`, `timeline` ||
|#

You can filter only by the `id`, `isAvailable`, and `isActive` fields with the operators `=`, `!=`, `in`. You can sort only by `id`.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.dictionary.dealtype.list`

{% endnote %}

The examples retrieve the first three deal types that can be deleted, in ascending sort order. For a different directory, replace the method name according to the table above and check the fields in `select`, `filter`, and `order` against the [Directory Fields](#fields) section.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","sort"],"filter":[["isSystem","=",false]],"order":{"sort":"ASC"},"pagination":{"limit":3}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.dictionary.dealtype.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","sort"],"filter":[["isSystem","=",false]],"order":{"sort":"ASC"},"pagination":{"limit":3},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.dictionary.dealtype.list
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DictionaryListResult = {
      items: {
        id: string
        name: string
        sort: number
      }[]
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<DictionaryListResult>(
        'crm.dictionary.dealtype.list',
        {
          select: ['id', 'name', 'sort'],
          filter: [
            ['isSystem', '=', false],
          ],
          order: {
            sort: 'ASC',
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
        console.info('Deal types:', result.items)
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
      async function listDealTypes() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.dictionary.dealtype.list',
            {
              select: ['id', 'name', 'sort'],
              filter: [
                ['isSystem', '=', false],
              ],
              order: {
                sort: 'ASC',
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
          console.info('Deal types:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listDealTypes)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.dictionary.dealtype.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "select": ["id", "name", "sort"],
                "filter": [
                    ["isSystem", "=", False],
                ],
                "order": {"sort": "ASC"},
                "pagination": {"limit": 3},
            },
            prefer_version=3,
        )
        for item in response["result"]["items"]:
            print(item["id"], item["name"], item["sort"])
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
                'crm.dictionary.dealtype.list',
                [
                    'select' => ['id', 'name', 'sort'],
                    'filter' => [
                        ['isSystem', '=', false],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'crm.dictionary.dealtype.list',
        {
            select: ['id', 'name', 'sort'],
            filter: [
                ['isSystem', '=', false],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.dictionary.dealtype.list',
        [
            'select' => ['id', 'name', 'sort'],
            'filter' => [
                ['isSystem', '=', false],
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
    res, err := client.Core().Call(ctx, "crm.dictionary.dealtype.list", b24.Params{
    	"select": []string{"id", "name", "sort"},
    	"filter": []any{
    		[]any{"isSystem", "=", false},
    	},
    	"order": b24.Params{
    		"sort": "ASC",
    	},
    	"pagination": b24.Params{
    		"limit": 3,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.dictionary.dealtype.list: %w", err)
    }

    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("response has no items key")
    }

    var items []struct {
    	ID   string `json:"id"`
    	Name string `json:"name"`
    	Sort int    `json:"sort"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.Name, it.Sort)
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
                "id": "COMPLEX",
                "name": "Integrated sales",
                "sort": 20
            },
            {
                "id": "GOODS",
                "name": "Merchandise sales",
                "sort": 30
            },
            {
                "id": "SERVICES",
                "name": "Services",
                "sort": 40
            }
        ]
    },
    "time": {
        "start": 1793715427,
        "finish": 1793715427.786939,
        "duration": 0.7869389057159424,
        "processing": 0,
        "date_start": "2026-11-03T17:17:07+03:00",
        "date_finish": "2026-11-03T17:17:07+03:00",
        "operating_reset_at": 1793716020,
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
[`array`](../../../data-types.md) | Array of directory values on the current page. If no values match the filter or the page is beyond the end of the list, the array is empty ||
|| **result.items[]**
[`object`](../../../data-types.md) | Directory value. The set of fields is defined by the `select` parameter. [Field descriptions](#fields) ||
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
                "message": "This request requires data type \"int\" in field \"sort\".",
                "field": "sort"
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
|| `sort` | This request requires data type `int` in field `sort` | Pass a value of the field type in the filter condition ||
|| `#FIELD#` | DTO `#DTO#` in field `#FIELD#` requires attribute `Filterable` to perform this request | The field cannot be used in a filter. The fields available for filtering are listed in the [Directory Fields](#fields) section ||
|| `#FIELD#` | DTO `#DTO#` in field `#FIELD#` requires attribute `Sortable` to perform this request | The field cannot be used for sorting. The fields available for sorting are listed in the [Directory Fields](#fields) section ||
|#

#### Filter Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid filter `#FILTER#` | Check the condition from the error message, the operators, and `negative` support against the [Directory Fields](#fields) section ||
|| `-` | Invalid filter `Operator "between" requires an array only with 2 items.` | Pass an array of two values for `between`: `["sort", "between", [20, 40]]` ||
|#

#### Unknown Filter Operator

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown filter operator `#OPERATOR#` | Use the operators from the [Directory Fields](#fields) section. There is no `like` operator ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `#DTO#` | Check the field name in `select`, `filter`, or `order` against the [Directory Fields](#fields) section ||
|#

#### Sort Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid sort order `#ORDER#` | Pass `order` as an object, for example, `{"sort":"DESC"}`, with the `ASC` or `DESC` direction ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Incorrect `pagination` parameter value: `#PAGINATION#` | Pass a positive number in `limit` and a non-negative number in `offset` ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Call the method on behalf of a user with permission to view leads, deals, or other CRM objects ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.dictionary.dealtype.list`. Take the directory name from the table at the beginning of the page ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-dictionary-add-rest-v3.md)
- [{#T}](./crm-dictionary-update-rest-v3.md)
- [{#T}](./crm-dictionary-get-rest-v3.md)
- [{#T}](./crm-dictionary-delete-rest-v3.md)
