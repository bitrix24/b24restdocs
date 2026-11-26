# Get a List of Companies by Filter crm.company.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: user with "Read" access permission for companies

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use the REST 3.0 method [crm.company.list](../rest-v3/items/crm-entity-list-rest-v3.md).

{% endnote %}

The method `crm.company.list` returns a list of companies based on a filter. It is an implementation of the [list method](../../../settings/how-to-call-rest-api/list-methods-pecularities.md) for companies.

The method selects only companies from the general category. A filter by `CATEGORY_ID` does not change this behavior. Retrieve companies from other categories using [crm.item.list](../universal/crm-item-list.md) with `entityTypeId=4` and `categoryId`.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`string[]`](../../data-types.md) | List of fields to limit the selection.

You can use masks for selection:
- `'*'` — to select all fields, excluding custom and multiple fields
- `'UF_*'` — to select all custom fields excluding multiple ones

There is no mask for selecting multiple fields. To select multiple fields, specify the required ones in the selection list — `PHONE`, `EMAIL`, and so on.

You can find the list of available fields for selection using the [crm.company.fields](crm-company-fields.md) method.

By default, all fields are returned — `'*'` + custom fields — `'UF_*'`
||
|| **filter**
[`object`](../../data-types.md) | Object format:

```text
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

where:
- `field_n` — the name of the field by which the selection of elements will be filtered
- `value_n` — the filter value

You can add a prefix to the keys `field_n` to clarify the filter operation.
Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for a substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `*` — word search. The value is split into words, and each word must occur in the field as a substring, in any order. For non-string fields, it works as `=`
- `*=` — same as `*`
- `=` — equal, exact match. Used by default, except for the `PHONE`, `EMAIL`, `WEB`, and `IM` fields
- `!=` — not equal
- `!` — not equal

In the filter, the `LAST_COMMUNICATION_TIME` field accepts a date in ISO 8601 format, for example, `2026-01-01T00:00:00+01:00`.

For the `PHONE`, `EMAIL`, `WEB`, and `IM` fields, a filter without a prefix searches for values that start with the passed string.

The `LIKE` filter does not work with fields of type `crm_status`, `crm_company` — for example, `COMPANY_TYPE`.

You can find the list of available fields for filtering using the [crm.company.fields](crm-company-fields.md) method.

The `logic` key in the filter is not supported. For complex logic in the filter, use the [crm.item.list](../universal/crm-item-list.md) method.
||
|| **order**
[`object`](../../data-types.md) | Object format:

```text
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```
where:
- `field_n` — the name of the field by which the selection of companies will be sorted
- `value_n` — a `string` value equal to:
    - `ASC` — ascending sort
    - `DESC` — descending sort

You can find the list of available fields for sorting using the [crm.company.fields](crm-company-fields.md) method
||
|| **start**
[`integer`](../../data-types.md) | Parameter for managing pagination.

A single page contains no more than 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the number of the desired page.

Pass `-1` to retrieve the first batch without counting the total. In this mode, `total` is `0` and `next` is not returned.
||
|#

## Code Examples

{% include [Example Note](../../../_includes/examples.md) %}

Retrieve companies:
- sorted by creation date
- with selected fields: title, responsible person, phone
- filtered by company type `CUSTOMER` and creation date from 2025-01-01

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ORDER":{"DATE_CREATE":"ASC"},"FILTER":{"COMPANY_TYPE":"CUSTOMER",">=DATE_CREATE":"2025-01-01"},"SELECT":["TITLE","ASSIGNED_BY_ID","PHONE"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ORDER":{"DATE_CREATE":"ASC"},"FILTER":{"COMPANY_TYPE":"CUSTOMER",">=DATE_CREATE":"2025-01-01"},"SELECT":["TITLE","ASSIGNED_BY_ID","PHONE"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.company.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each company object returned in result[]
    type CrmCompanyListItem = {
      ID: string
      TITLE: string
      ASSIGNED_BY_ID: string
      PHONE?: CrmCompanyPhone[]
    }

    type CrmCompanyPhone = {
      ID: string
      VALUE_TYPE: string
      VALUE: string
      TYPE_ID: string
    }

    try {
      // crm.company.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<CrmCompanyListItem[]>({
        method: 'crm.company.list',
        params: {
          order: { DATE_CREATE: 'ASC' },
          filter: { COMPANY_TYPE: 'CUSTOMER', '>=DATE_CREATE': '2025-01-01' },
          select: ['TITLE', 'ASSIGNED_BY_ID', 'PHONE'],
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Companies on this page:', result.length, result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listCompanies() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.company.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'crm.company.list',
            params: {
              order: { DATE_CREATE: 'ASC' },
              filter: { COMPANY_TYPE: 'CUSTOMER', '>=DATE_CREATE': '2025-01-01' },
              select: ['TITLE', 'ASSIGNED_BY_ID', 'PHONE'],
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Companies on this page:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listCompanies)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.company.list',
                [
                    'order' => ['DATE_CREATE' => 'ASC'],
                    'filter' => ['COMPANY_TYPE' => 'CUSTOMER', '>=DATE_CREATE' => '2025-01-01'],
                    'select' => ['TITLE', 'ASSIGNED_BY_ID', 'PHONE'],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        
        if ($result->more()) {
            $result->next();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching company list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.company.list",
        {
            order: { "DATE_CREATE": "ASC" },
            filter: { "COMPANY_TYPE": "CUSTOMER", ">=DATE_CREATE": "2025-01-01" },
            select: [ "TITLE", "ASSIGNED_BY_ID", "PHONE" ]
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.dir(result.data());
                if(result.more())
                    result.next();
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.company.list',
        [
            'order' => ['DATE_CREATE' => 'ASC'],
            'filter' => ['COMPANY_TYPE' => 'CUSTOMER', '>=DATE_CREATE' => '2025-01-01'],
            'select' => ['TITLE', 'ASSIGNED_BY_ID', 'PHONE'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.company.list", b24.Params{
    	"order": b24.Params{
    		"DATE_CREATE": "ASC",
    	},
    	"filter": b24.Params{
    		"COMPANY_TYPE":  "CUSTOMER",
    		">=DATE_CREATE": "2025-01-01",
    	},
    	"select": []string{"TITLE", "ASSIGNED_BY_ID", "PHONE"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.company.list: %w", err)
    }

    var items []struct {
    	Title        string `json:"TITLE"`
    	AssignedByID b24.ID `json:"ASSIGNED_BY_ID"`
    	ID           b24.ID `json:"ID"`
    	Phone        []struct {
    		ID        string `json:"ID"`
    		ValueType string `json:"VALUE_TYPE"`
    		Value     string `json:"VALUE"`
    		TypeID    string `json:"TYPE_ID"`
    	} `json:"PHONE"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.Title, it.AssignedByID, it.Phone)
    }

    // Total and Next are filled in by list methods; for a full
    // list traversal, use client.Core().Pages and Scan.
    if res.Total != nil {
    	fmt.Println("total:", *res.Total)
    }
    ```

{% endlist %}

## Response Handling

System fields hidden from the user by [system field visibility settings](../index.md#system-field-visibility) are not returned, even if specified in `select`. `filter` conditions and `order` keys on hidden fields are silently discarded without an error.

HTTP Status: **200**

```json
{
    "result": [
        {
            "TITLE": "Crossroads",
            "ASSIGNED_BY_ID": "811",
            "ID": "2919",
            "PHONE": [
                {
                    "ID": "8303",
                    "VALUE_TYPE": "WORK",
                    "VALUE": "+19998887766",
                    "TYPE_ID": "PHONE"
                }
            ]
        }
    ],
    "total": 1,
    "time": {
        "start": 1769498859,
        "finish": 1769498859.948139,
        "duration": 0.948138952255249,
        "processing": 0,
        "date_start": "2026-01-27T10:27:39+01:00",
        "date_finish": "2026-01-27T10:27:39+01:00",
        "operating_reset_at": 1769499459,
        "operating": 0.1621239185333252
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../../data-types.md) | Array of companies matching the filter. The set of fields depends on `select`; the field descriptions can be retrieved using the method [crm.company.fields](crm-company-fields.md) ||
|| **total**
[`integer`](../../data-types.md) | Total number of companies found ||
|| **next**
[`integer`](../../data-types.md) | The value of `start` for the next request to retrieve the next page. Returned if there are more records after the current page and `start` is not `-1` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Element of the result Array

Each element is a company object. For example, `ID` is the company identifier, `TITLE` is the name, and `ASSIGNED_BY_ID` is the identifier of the responsible user. The `PHONE`, `EMAIL`, `WEB`, and `IM` fields, if selected in `select`, contain arrays of `crm_multifield` objects.

#### Object crm_multifield

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Identifier of the multiple field value ||
|| **VALUE_TYPE**
[`string`](../../data-types.md) | Value type, for example `WORK` for a work phone ||
|| **VALUE**
[`string`](../../data-types.md) | Value: phone number, email address, website, or messenger ||
|| **TYPE_ID**
[`string`](../../data-types.md) | Field type: `PHONE`, `EMAIL`, `WEB`, or `IM` ||
|#

The full list of value types is available in the [crm_multifield](../data-types.md#crm_multifield) description.

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | `Access denied.` | The user does not have "Read" access permission for companies ||
|| `400` | Empty value | `Parameter 'order' must be array.` | The `order` parameter is not an object ||
|| `400` | Empty value | `Parameter 'filter' must be array.` | The `filter` parameter is not an object ||
|| `400` | Empty value | `Failed to get list. General error.` | An unknown error occurred ||
|#

{% include [System Errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-company-add.md)
- [{#T}](./crm-company-update.md)
- [{#T}](./crm-company-get.md)
- [{#T}](./crm-company-delete.md)
- [{#T}](./crm-company-fields.md)
- [{#T}](../../../tutorials/crm/how-to-get-lists/search-by-phone-and-email.md)