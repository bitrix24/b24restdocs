# Get a List of Recurring Deal Template Settings crm.deal.recurring.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "read" access permission for deals

The method `crm.deal.recurring.list` returns a list of recurring deal template settings based on the provided filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **order**
[`object`](../../../data-types.md) | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

where:
- `field_n` — the name of the field to sort by
- `value_n` — sort direction: `ASC` or `DESC`

The list of fields available for sorting can be obtained using the method [crm.deal.recurring.fields](./crm-deal-recurring-fields.md) ||
|| **filter**
[`object`](../../../data-types.md) | Filter object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

where:
- `field_n` — the name of the field to filter the selection of elements
- `value_n` — filter value

You can add a prefix to the keys `field_n` to specify the filter operation.
Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol does not need to be included in the filter value. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol must be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `=` — equals, exact match (used by default)
- `!=` — not equal
- `!` — not equal

The list of fields available for filtering can be obtained using the method [crm.deal.recurring.fields](./crm-deal-recurring-fields.md).

The `PARAMS` field in the filter is ignored and does not affect the selection result || 
|| **start**
[`integer`](../../../data-types.md) | Pagination parameter.

The page size is fixed: `50` records.

The formula for obtaining the N-th page:
`start = (N - 1) * 50` ||
|#

## Code Examples

{% include [Example Notes](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"deal_id":"ASC"},"filter":{">COUNTER_REPEAT":0}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.recurring.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"deal_id":"ASC"},"filter":{">COUNTER_REPEAT":0},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.recurring.list
    ```

- JS

    ```js
    // callListMethod: retrieves all data at once. Suitable for small selections.
    try {
      const response = await $b24.callListMethod(
        'crm.deal.recurring.list',
        {
          order: { deal_id: 'ASC' },
          filter: { '>COUNTER_REPEAT': 0 }
        }
      );
      const items = response.getData() || [];
      for (const item of items) {
        console.log(item);
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // fetchListMethod: retrieves data in parts (iterator).
    try {
      const iterator = $b24.fetchListMethod(
        'crm.deal.recurring.list',
        {
          order: { deal_id: 'ASC' },
          filter: { '>COUNTER_REPEAT': 0 }
        },
        'id'
      );
      for await (const page of iterator) {
        for (const item of page) {
          console.log(item);
        }
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // callMethod: manual pagination using the start parameter.
    try {
      const response = await $b24.callMethod(
        'crm.deal.recurring.list',
        {
          order: { deal_id: 'ASC' },
          filter: { '>COUNTER_REPEAT': 0 }
        },
        0
      );
      const result = response.getData().result || [];
      for (const item of result) {
        console.log(item);
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.deal.recurring.list',
                [
                    'order' => ['deal_id' => 'ASC'],
                    'filter' => ['>COUNTER_REPEAT' => 0],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching recurring deals: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.deal.recurring.list',
        {
            order: { deal_id: 'ASC' },
            filter: { '>COUNTER_REPEAT': 0 }
        },
        function(result)
        {
            if (result.error())
                console.error(result.error());
            else
            {
                console.info(result.data());
                if (result.more())
                    result.next();
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.recurring.list',
        [
            'order' => ['deal_id' => 'ASC'],
            'filter' => ['>COUNTER_REPEAT' => 0],
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
            "id": "1",
            "deal_id": "575",
            "based_id": "573",
            "ACTIVE": "N",
            "category_id": "1",
            "IS_LIMIT": "T",
            "COUNTER_REPEAT": "2",
            "LIMIT_REPEAT": "2",
            "LIMIT_DATE": "",
            "START_DATE": "2020-06-15T03:00:00+02:00",
            "NEXT_EXECUTION": "",
            "LAST_EXECUTION": "2020-06-17T01:00:00+02:00",
            "PARAMS": {
                "MODE": "multiple",
                "SINGLE_BEFORE_START_DATE_TYPE": "day",
                "SINGLE_BEFORE_START_DATE_VALUE": 0,
                "MULTIPLE_TYPE": "day",
                "MULTIPLE_INTERVAL": 1,
                "OFFSET_BEGINDATE_TYPE": "day",
                "OFFSET_BEGINDATE_VALUE": 1,
                "OFFSET_CLOSEDATE_TYPE": "day",
                "OFFSET_CLOSEDATE_VALUE": 1
            }
        },
        {
            "id": "5",
            "deal_id": "6555",
            "based_id": "6531",
            "ACTIVE": "Y",
            "category_id": "9",
            "IS_LIMIT": "N",
            "COUNTER_REPEAT": "471",
            "LIMIT_REPEAT": null,
            "LIMIT_DATE": "",
            "START_DATE": "2024-11-21T03:00:00+02:00",
            "NEXT_EXECUTION": "2026-03-07T01:00:00+02:00",
            "LAST_EXECUTION": "2026-03-06T01:00:00+02:00",
            "PARAMS": {
                "MODE": "multiple",
                "SINGLE_BEFORE_START_DATE_TYPE": "day",
                "SINGLE_BEFORE_START_DATE_VALUE": 0,
                "MULTIPLE_TYPE": "day",
                "MULTIPLE_INTERVAL": 1,
                "OFFSET_BEGINDATE_TYPE": "day",
                "OFFSET_BEGINDATE_VALUE": 0,
                "OFFSET_CLOSEDATE_TYPE": "day",
                "OFFSET_CLOSEDATE_VALUE": 0
            }
        }
    ],
    "total": 5,
    "time": {
        "start": 1772757008,
        "finish": 1772757008.649235,
        "duration": 0.6492350101470947,
        "processing": 0,
        "date_start": "2026-03-06T03:30:08+02:00",
        "date_finish": "2026-03-06T03:30:08+02:00",
        "operating_reset_at": 1772757608,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`recurring_deal[]`](./crm-deal-recurring-get.md#recurring-deal) | Array of recurring deal template settings.

Field descriptions are provided in the method [crm.deal.recurring.get](./crm-deal-recurring-get.md) ||
|| **total**
[`integer`](../../../data-types.md) | Total number of records found ||
|| **next**
[`integer`](../../../data-types.md) | Offset for the next page request.

This field is present if there is a next page of results ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `Access denied` | Insufficient permissions to read deals ||
|| `Parameter 'order' must be array` | The `order` parameter is not an object ||
|| `Parameter 'filter' must be array` | The `filter` parameter is not an object ||
|| `Failed to get list. General error` | Unable to retrieve the list due to an internal error ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-recurring-fields.md)
- [{#T}](./crm-deal-recurring-get.md)
- [{#T}](./crm-deal-recurring-add.md)
- [{#T}](./crm-deal-recurring-update.md)
- [{#T}](./crm-deal-recurring-delete.md)
- [{#T}](./crm-deal-recurring-expose.md)