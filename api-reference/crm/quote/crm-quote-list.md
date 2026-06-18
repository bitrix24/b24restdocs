# Get a List of Estimates by Filter crm.quote.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "read" access permission for estimates

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [crm.item.list](../universal/crm-item-list.md).

{% endnote %}

The method `crm.quote.list` returns a list of estimates based on a filter.

This method implements the [list method](../../../settings/how-to-call-rest-api/list-methods-pecularities.md) for estimates.

## Method Parameters

{% include [Note on Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`string[]`](../data-types.md) | A list of fields to return in the response.

You can use masks for selection:
- `'*'` — to select all standard fields (excluding custom and multiple fields),
- `'UF_*'` — to select all custom fields.

A list of available fields for selection can be obtained using the [crm.quote.fields](./crm-quote-fields.md) method.

By default, all standard fields and custom fields are returned (`'*'` + `'UF_*'`) ||
|| **filter**
[`object`](../data-types.md) | An object in the format:

```json
{
    "field_1": "value_1",
    "field_2": "value_2",
    "...": "..."
}
```

where:
- `field_n` — the name of the field to filter the selection,
- `value_n` — the filter value.

The filter key format: `<operator><field>`.  
Example: `>=DATE_CREATE`, `@ASSIGNED_BY_ID`, `=%TITLE`.

Supported operators:
- `=` — equal (exact match, used by default)
- `!=` — not equal
- `!` — not equal
- `>` — greater than
- `>=` — greater than or equal to
- `<` — less than
- `<=` — less than or equal to
- `@` — IN (an array is passed in the value)
- `!@` — NOT IN (an array is passed in the value)
- `%` — LIKE, substring search
- `=%` — LIKE, pattern search
- `%=` — LIKE (similar to `=%`)

For `LIKE`:
- `=%TITLE: "%meb"` — substring anywhere
- `=%TITLE: "meb%"` — starts with `meb`
- `=%TITLE: "%meb"` — ends with `meb`

A list of available fields for filtering can be obtained using the [crm.quote.fields](./crm-quote-fields.md) method ||
|| **order**
[`object`](../data-types.md) | An object in the format:

```json
{
    "field_1": "ASC",
    "field_2": "DESC"
}
```

where:
- `field_n` — the sorting field,
- value:
  - `ASC` — ascending,
  - `DESC` — descending.

A list of available fields for sorting can be obtained using the [crm.quote.fields](./crm-quote-fields.md) method.

When sorting by `STATUS_ID`, the internal field `STATUS_SORT` is used ||
|| **start**
[`integer`](../data-types.md) | Pagination parameter.

Page size — `50` records.

Formula:

`start = (N - 1) * 50`, where `N` — page number ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

Select estimates:
- for the company with `COMPANY_ID = 1`,
- with the stage `SENT`,
- sorted by stage and ID,
- selecting fields: `ID`, `TITLE`, `STATUS_ID`, `OPPORTUNITY`, `CURRENCY_ID`, `ASSIGNED_BY_ID`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"STATUS_ID":"ASC","ID":"ASC"},"filter":{"=COMPANY_ID":1,"=STATUS_ID":"SENT"},"select":["ID","TITLE","STATUS_ID","OPPORTUNITY","CURRENCY_ID","ASSIGNED_BY_ID"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"STATUS_ID":"ASC","ID":"ASC"},"filter":{"=COMPANY_ID":1,"=STATUS_ID":"SENT"},"select":["ID","TITLE","STATUS_ID","OPPORTUNITY","CURRENCY_ID","ASSIGNED_BY_ID"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.quote.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each QuoteItem returned in result[]
    type QuoteItem = {
      ID: string
      TITLE: string
      STATUS_ID: string
      OPPORTUNITY: string
      CURRENCY_ID: string
      ASSIGNED_BY_ID: string
    }

    try {
      // crm.quote.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<QuoteItem[]>({
        method: 'crm.quote.list',
        params: {
          order: { STATUS_ID: 'ASC', ID: 'ASC' },
          filter: { '=COMPANY_ID': 1, '=STATUS_ID': 'SENT' },
          select: ['ID', 'TITLE', 'STATUS_ID', 'OPPORTUNITY', 'CURRENCY_ID', 'ASSIGNED_BY_ID'],
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Quotes on this page:', result.length, result)
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
      async function fetchQuoteList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.quote.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'crm.quote.list',
            params: {
              order: { STATUS_ID: 'ASC', ID: 'ASC' },
              filter: { '=COMPANY_ID': 1, '=STATUS_ID': 'SENT' },
              select: ['ID', 'TITLE', 'STATUS_ID', 'OPPORTUNITY', 'CURRENCY_ID', 'ASSIGNED_BY_ID'],
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
          console.info('Quotes on this page:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchQuoteList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.quote.list',
                [
                    'order' => [
                        'STATUS_ID' => 'ASC',
                        'ID' => 'ASC',
                    ],
                    'filter' => [
                        '=COMPANY_ID' => 1,
                        '=STATUS_ID' => 'SENT',
                    ],
                    'select' => [
                        'ID',
                        'TITLE',
                        'STATUS_ID',
                        'OPPORTUNITY',
                        'CURRENCY_ID',
                        'ASSIGNED_BY_ID',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo '<pre>';
        print_r($result);
        echo '</pre>';

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching quote list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.quote.list',
        {
            order: { STATUS_ID: 'ASC', ID: 'ASC' },
            filter: { '=COMPANY_ID': 1, '=STATUS_ID': 'SENT' },
            select: ['ID', 'TITLE', 'STATUS_ID', 'OPPORTUNITY', 'CURRENCY_ID', 'ASSIGNED_BY_ID'],
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

    $result = CRest::call(
        'crm.quote.list',
        [
            'order' => [
                'STATUS_ID' => 'ASC',
                'ID' => 'ASC',
            ],
            'filter' => [
                '=COMPANY_ID' => 1,
                '=STATUS_ID' => 'SENT',
            ],
            'select' => [
                'ID',
                'TITLE',
                'STATUS_ID',
                'OPPORTUNITY',
                'CURRENCY_ID',
                'ASSIGNED_BY_ID',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "ID": "9",
            "TITLE": "Latest version of our product",
            "STATUS_ID": "SENT",
            "OPPORTUNITY": "45000.00",
            "CURRENCY_ID": "USD",
            "ASSIGNED_BY_ID": "7"
        },
        {
            "ID": "43",
            "TITLE": "Estimate for furniture supply",
            "STATUS_ID": "SENT",
            "OPPORTUNITY": "150000.00",
            "CURRENCY_ID": "USD",
            "ASSIGNED_BY_ID": "1"
        }
    ],
    "total": 2,
    "time": {
        "start": 1773413037,
        "finish": 1773413037.105712,
        "duration": 0.1057119369506836,
        "processing": 0,
        "date_start": "2026-03-13T17:43:57+01:00",
        "date_finish": "2026-03-13T17:43:57+01:00",
        "operating_reset_at": 1773413637,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../data-types.md) | An array of estimates. The composition of fields depends on the `select` parameter ||
|| **total**
[`integer`](../data-types.md) | The total number of records found ||
|| **next**
[`integer`](../data-types.md) | The value for the `start` parameter in the next request.

The `next` parameter is returned if the number of items in the selection exceeds `50` ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | `Parameter 'order' must be array.` | An object was not passed in `order` ||
|| `-` | `Parameter 'filter' must be array.` | An object was not passed in `filter` ||
|| `-` | `Access denied.` | The user does not have permission to read estimates ||
|| `-` | `Failed to get list. General error.` | General error executing the request ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-quote-add.md)
- [{#T}](./crm-quote-update.md)
- [{#T}](./crm-quote-get.md)
- [{#T}](./crm-quote-delete.md)
- [{#T}](./crm-quote-fields.md)