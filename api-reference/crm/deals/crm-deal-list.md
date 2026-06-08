# Get a List of Deals crm.deal.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: any user with "read" access permission for deals

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [crm.item.list](../universal/crm-item-list.md).

{% endnote %}

The method `crm.deal.list` returns a list of deals based on a filter. It is an implementation of the list method for deals.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`string[]`](../../data-types.md) | A list of fields that should be populated for deals in the selection.

The following masks can be used in the selection:
- `'*'` — to select all fields (excluding custom and multiple fields)
- `'UF_*'` — to select all custom fields (excluding multiple fields)

You can find the list of available fields for selection using the method [crm.deal.fields](./crm-deal-fields.md). 
The method does not support the field `CONTACT_IDS`; to get deals with a list of contacts, use the method [crm.item.list](../universal/crm-item-list.md).

By default, all fields are taken — `'*'` + Custom fields — `'UF_*'`
||
|| **filter**
[`object`](../../data-types.md) | Object format:

```
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

You can add a prefix to the keys `field_n` to specify the filter operation.
Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol in the filter value should not be included. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

The LIKE filter does not work with fields of type `crm_status`, `crm_contact`, `crm_company` (deal type `TYPE_ID`, stage `STAGE_ID`, etc.).

You can find the list of available fields for filtering using the method [crm.deal.fields](./crm-deal-fields.md). 

The filter does not support the field `CONTACT_IDS`; to filter by contacts, use the method [crm.item.list](../universal/crm-item-list.md)
||
|| **order**
[`object`](../../data-types.md) | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

where:
- `field_n` — the name of the field by which the selection of deals will be sorted
- `value_n` — a `string` value, equal to:
    - `ASC` — ascending sort
    - `DESC` — descending sort

You can find the list of available fields for sorting using the method [crm.deal.fields](./crm-deal-fields.md)
||
|| **start**
[`integer`](../../data-types.md)  | This parameter is used to manage pagination.

The page size for results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the desired page number
||
|#

Also, see the description of [list methods](../../../settings/how-to-call-rest-api/list-methods-pecularities.md).

{% note tip "Related methods and topics" %}

[{#T}](./recurring-deals/crm-deal-recurring-list.md)

{% endnote %}

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

Get a list of deals where:
1. the funnel ID is `1`
2. the deal type is `COMPLEX`
3. the title ends with `a`
4. the stage is `C1:NEW`
5. the amount is greater than 10000 but less than or equal to 20000
6. manual mode for amount calculation is enabled
7. the responsible person is either the user with `id = 1` or the user with `id = 6`
8. the deal was created at least 6 months ago

Set the following sort order for this selection: title and amount in ascending order.

For clarity, select only the necessary fields:
- Identifier `ID`
- Title `TITLE`
- Deal type `TYPE_ID`
- Funnel ID `CATEGORY_ID`
- Stage `STAGE_ID`
- Amount `OPPORTUNITY`
- Is manual mode enabled `IS_MANUAL_OPPORTUNITY`
- Responsible `ASSIGNED_BY_ID`
- Creation date `DATE_CREATE`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"SELECT":["ID","TITLE","TYPE_ID","CATEGORY_ID","STAGE_ID","OPPORTUNITY","IS_MANUAL_OPPORTUNITY","ASSIGNED_BY_ID","DATE_CREATE"],"FILTER":{"=%TITLE":"%a","CATEGORY_ID":1,"TYPE_ID":"COMPLEX","STAGE_ID":"C1:NEW",">OPPORTUNITY":10000,"<=OPPORTUNITY":20000,"IS_MANUAL_OPPORTUNITY":"Y","@ASSIGNED_BY_ID":[1,6],">DATE_CREATE":"'"$(date --date='-6 months' +%Y-%m-%d)"'"},"ORDER":{"TITLE":"ASC","OPPORTUNITY":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"SELECT":["ID","TITLE","TYPE_ID","CATEGORY_ID","STAGE_ID","OPPORTUNITY","IS_MANUAL_OPPORTUNITY","ASSIGNED_BY_ID","DATE_CREATE"],"FILTER":{"=%TITLE":"%a","CATEGORY_ID":1,"TYPE_ID":"COMPLEX","STAGE_ID":"C1:NEW",">OPPORTUNITY":10000,"<=OPPORTUNITY":20000,"IS_MANUAL_OPPORTUNITY":"Y","@ASSIGNED_BY_ID":[1,6],">DATE_CREATE":"'"$(date --date='-6 months' +%Y-%m-%d)"'"},"ORDER":{"TITLE":"ASC","OPPORTUNITY":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each deal returned in result[] (subset shaped by `select`)
    type CrmDealListItem = {
      ID: string
      TITLE: string
      TYPE_ID: string
      CATEGORY_ID: string
      STAGE_ID: string
      OPPORTUNITY: string
      IS_MANUAL_OPPORTUNITY: string
      ASSIGNED_BY_ID: string
      DATE_CREATE: ISODate | null
    }

    const now = new Date()
    const sixMonthAgo = new Date()
    sixMonthAgo.setMonth(now.getMonth() - 6)

    try {
      // crm.deal.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<CrmDealListItem[]>({
        method: 'crm.deal.list',
        params: {
          select: [
            'ID',
            'TITLE',
            'TYPE_ID',
            'CATEGORY_ID',
            'STAGE_ID',
            'OPPORTUNITY',
            'IS_MANUAL_OPPORTUNITY',
            'ASSIGNED_BY_ID',
            'DATE_CREATE',
          ],
          filter: {
            '=%TITLE': '%a',
            CATEGORY_ID: 1,
            TYPE_ID: 'COMPLEX',
            STAGE_ID: 'C1:NEW',
            '>OPPORTUNITY': 10000,
            '<=OPPORTUNITY': 20000,
            IS_MANUAL_OPPORTUNITY: 'Y',
            '@ASSIGNED_BY_ID': [1, 6],
            '>DATE_CREATE': sixMonthAgo,
          },
          order: {
            TITLE: 'ASC',
            OPPORTUNITY: 'ASC',
          },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Deals on this page:', result.length, result)
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
      async function listDeals() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const now = new Date()
          const sixMonthAgo = new Date()
          sixMonthAgo.setMonth(now.getMonth() - 6)

          // crm.deal.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'crm.deal.list',
            params: {
              select: [
                'ID',
                'TITLE',
                'TYPE_ID',
                'CATEGORY_ID',
                'STAGE_ID',
                'OPPORTUNITY',
                'IS_MANUAL_OPPORTUNITY',
                'ASSIGNED_BY_ID',
                'DATE_CREATE',
              ],
              filter: {
                '=%TITLE': '%a',
                CATEGORY_ID: 1,
                TYPE_ID: 'COMPLEX',
                STAGE_ID: 'C1:NEW',
                '>OPPORTUNITY': 10000,
                '<=OPPORTUNITY': 20000,
                IS_MANUAL_OPPORTUNITY: 'Y',
                '@ASSIGNED_BY_ID': [1, 6],
                '>DATE_CREATE': sixMonthAgo,
              },
              order: {
                TITLE: 'ASC',
                OPPORTUNITY: 'ASC',
              },
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
          console.info('Deals on this page:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listDeals)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.deal.list',
                [
                    'select' => [
                        'ID',
                        'TITLE',
                        'TYPE_ID',
                        'CATEGORY_ID',
                        'STAGE_ID',
                        'OPPORTUNITY',
                        'IS_MANUAL_OPPORTUNITY',
                        'ASSIGNED_BY_ID',
                        'DATE_CREATE',
                    ],
                    'filter' => [
                        '=%TITLE'              => '%a',
                        'CATEGORY_ID'          => 1,
                        'TYPE_ID'              => 'COMPLEX',
                        'STAGE_ID'             => 'C1:NEW',
                        '>OPPORTUNITY'         => 10000,
                        '<=OPPORTUNITY'        => 20000,
                        'IS_MANUAL_OPPORTUNITY' => 'Y',
                        '@ASSIGNED_BY_ID'      => [1, 6],
                        '>DATE_CREATE'         => $sixMonthAgo,
                    ],
                    'order' => [
                        'TITLE'       => 'ASC',
                        'OPPORTUNITY' => 'ASC',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching deal list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    const now = new Date();
    const sixMonthAgo = new Date();
    sixMonthAgo.setMonth(now.getMonth() - 6);
    
    BX24.callMethod(
        'crm.deal.list',
        {
            select: [
                'ID',
                'TITLE',
                'TYPE_ID',
                'CATEGORY_ID',
                'STAGE_ID',
                'OPPORTUNITY',
                'IS_MANUAL_OPPORTUNITY',
                'ASSIGNED_BY_ID',
                'DATE_CREATE',
            ],
            filter: {
                '=%TITLE': '%a',
                CATEGORY_ID: 1,
                TYPE_ID: 'COMPLEX',
                STAGE_ID: 'C1:NEW',
                '>OPPORTUNITY': 10000,
                '<=OPPORTUNITY': 20000,
                IS_MANUAL_OPPORTUNITY: 'Y',
                '@ASSIGNED_BY_ID': [1, 6],
                '>DATE_CREATE': sixMonthAgo,
            },
            order: {
                TITLE: 'ASC',
                OPPORTUNITY: 'ASC',
            },
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $sixMonthAgo = (new DateTime())->modify('-6 months')->format('Y-m-d');

    $result = CRest::call(
        'crm.deal.list',
        [
            'SELECT' => [
                'ID',
                'TITLE',
                'TYPE_ID',
                'CATEGORY_ID',
                'STAGE_ID',
                'OPPORTUNITY',
                'IS_MANUAL_OPPORTUNITY',
                'ASSIGNED_BY_ID',
                'DATE_CREATE',
            ],
            'FILTER' => [
                '=%TITLE' => '%a',
                'CATEGORY_ID' => 1,
                'TYPE_ID' => 'COMPLEX',
                'STAGE_ID' => 'C1:NEW',
                '>OPPORTUNITY' => 10000,
                '<=OPPORTUNITY' => 20000,
                'IS_MANUAL_OPPORTUNITY' => 'Y',
                '@ASSIGNED_BY_ID' => [1, 6],
                '>DATE_CREATE' => $sixMonthAgo,
            ],
            'ORDER' => [
                'TITLE' => 'ASC',
                'OPPORTUNITY' => 'ASC',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.deal.list(
            select=["ID", "TITLE", "STAGE_ID", "OPPORTUNITY", "ASSIGNED_BY_ID", "DATE_CREATE"],
            filter={">OPPORTUNITY": 1000, "!STAGE_ID": "WON", "=OPENED": "Y"},
            order={"DATE_CREATE": "DESC", "ID": "DESC"},
            start=0,
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

    Example `as_list`

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.deal.list(
            select=["ID", "TITLE", "STAGE_ID"],
            filter={"!STAGE_ID": "LOSE"},
            order={"ID": "ASC"},
        ).as_list().response
        result = bitrix_response.result
        for item in result:
            print(item)
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

    Example `as_list_fast`

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.deal.list(
            select=["ID", "TITLE", "STAGE_ID"],
            filter={"!STAGE_ID": "LOSE"},
            order={"ID": "DESC"},
        ).as_list_fast(descending=True).response
        result = bitrix_response.result
        for item in result:
            print(item)
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
{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "ID": "37",
            "TITLE": "[A] Deal",
            "TYPE_ID": "COMPLEX",
            "CATEGORY_ID": "1",
            "STAGE_ID": "C1:NEW",
            "OPPORTUNITY": "19999.99",
            "IS_MANUAL_OPPORTUNITY": "Y",
            "ASSIGNED_BY_ID": "1",
            "DATE_CREATE": "2024-09-02T18:37:18+02:00"
        },
        {
            "ID": "38",
            "TITLE": "[A] Deal",
            "TYPE_ID": "COMPLEX",
            "CATEGORY_ID": "1",
            "STAGE_ID": "C1:NEW",
            "OPPORTUNITY": "20000.00",
            "IS_MANUAL_OPPORTUNITY": "Y",
            "ASSIGNED_BY_ID": "6",
            "DATE_CREATE": "2024-09-02T18:37:38+02:00"
        },
        {
            "ID": "39",
            "TITLE": "[B] Sale",
            "TYPE_ID": "COMPLEX",
            "CATEGORY_ID": "1",
            "STAGE_ID": "C1:NEW",
            "OPPORTUNITY": "12500.00",
            "IS_MANUAL_OPPORTUNITY": "Y",
            "ASSIGNED_BY_ID": "1",
            "DATE_CREATE": "2024-04-09T23:11:01+02:00"
        },
        {
            "ID": "40",
            "TITLE": "[B] Deal",
            "TYPE_ID": "COMPLEX",
            "CATEGORY_ID": "1",
            "STAGE_ID": "C1:NEW",
            "OPPORTUNITY": "13500.00",
            "IS_MANUAL_OPPORTUNITY": "Y",
            "ASSIGNED_BY_ID": "6",
            "DATE_CREATE": "2024-08-08T19:00:14+02:00"
        },
        {
            "ID": "41",
            "TITLE": "[V] Deal",
            "TYPE_ID": "COMPLEX",
            "CATEGORY_ID": "1",
            "STAGE_ID": "C1:NEW",
            "OPPORTUNITY": "11500.00",
            "IS_MANUAL_OPPORTUNITY": "Y",
            "ASSIGNED_BY_ID": "6",
            "DATE_CREATE": "2024-05-08T09:38:23+02:00"
        },
        {
            "ID": "42",
            "TITLE": "[S] Deal",
            "TYPE_ID": "COMPLEX",
            "CATEGORY_ID": "1",
            "STAGE_ID": "C1:NEW",
            "OPPORTUNITY": "18500.00",
            "IS_MANUAL_OPPORTUNITY": "Y",
            "ASSIGNED_BY_ID": "6",
            "DATE_CREATE": "2024-07-02T15:38:32+02:00"
        }
    ],
    "total": 6,
    "time": {
        "start": 1725292115.026221,
        "finish": 1725292115.907058,
        "duration": 0.8808369636535645,
        "processing": 0.2484450340270996,
        "date_start": "2024-09-02T17:48:35+02:00",
        "date_finish": "2024-09-02T17:48:35+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`deal[]`](crm-deal-get.md#deal) | The root element of the response. Contains an array of objects with information about the deal fields. 

Note that the structure of the fields may change due to the `select` parameter ||
|| **total**
[`integer`](../../data-types.md) | The total number of found items ||
|| **next**
[`integer`](../../data-types.md) | Contains the value to be passed in the next request in the `start` parameter to get the next batch of data.

The `next` parameter appears in the response if the number of items matching your request exceeds `50` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Parameter 'filter' must be array."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-`     | `Access denied` | The user does not have permission to "read" deals ||
|| `-`     | `Parameter 'order' must be array` | A non-object was passed to the `order` parameter ||
|| `-`     | `Parameter 'filter' must be array` | A non-object was passed to the `filter` parameter ||
|| `-`     | `Failed to get list. General error` | An unknown error occurred ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-add.md)
- [{#T}](./crm-deal-update.md)
- [{#T}](./crm-deal-get.md)
- [{#T}](./crm-deal-delete.md)
- [{#T}](./crm-deal-fields.md)
- [{#T}](../../../tutorials/crm/how-to-add-crm-objects/how-to-add-objects-with-crm-mode.md)
