# Get Recurring Deal Template Settings by ID crm.deal.recurring.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "read" access permission for deals

The method `crm.deal.recurring.get` returns the fields of the recurring deal template settings by its identifier.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id^*^**
[`integer`](../../../data-types.md) | Identifier of the recurring deal template settings.

The identifier can be obtained using the methods [crm.deal.recurring.list](./crm-deal-recurring-list.md) or [crm.deal.recurring.add](./crm-deal-recurring-add.md) ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.recurring.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.recurring.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.deal.recurring.get',
    		{
    			id: 15,
    		}
    	);
    
    	const result = response.getData().result;
    	console.info('Template settings:', result);
    }
    catch (error)
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.deal.recurring.get',
                [
                    'id' => 15,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Recurring settings: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting recurring settings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.deal.recurring.get',
        {
            id: 15
        },
        function(result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.recurring.get',
        [
            'id' => 15
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
    "result": {
        "ID": "25",
        "DEAL_ID": "7591",
        "BASED_ID": "7577",
        "ACTIVE": "Y",
        "CATEGORY_ID": "2",
        "IS_LIMIT": "D",
        "COUNTER_REPEAT": null,
        "LIMIT_REPEAT": null,
        "LIMIT_DATE": "2027-03-05T03:00:00+01:00",
        "START_DATE": "2026-04-05T03:00:00+01:00",
        "NEXT_EXECUTION": "2026-04-05T01:00:00+01:00",
        "LAST_EXECUTION": "",
        "PARAMS": {
            "MODE": "multiple",
            "SINGLE_BEFORE_START_DATE_TYPE": null,
            "SINGLE_BEFORE_START_DATE_VALUE": 0,
            "MULTIPLE_TYPE": "month",
            "MULTIPLE_INTERVAL": 1,
            "OFFSET_BEGINDATE_TYPE": "day",
            "OFFSET_BEGINDATE_VALUE": 1,
            "OFFSET_CLOSEDATE_TYPE": "month",
            "OFFSET_CLOSEDATE_VALUE": 2
        }
    },
    "time": {
        "start": 1772753726,
        "finish": 1772753726.940512,
        "duration": 0.94051194190979,
        "processing": 0,
        "date_start": "2026-03-06T02:35:26+01:00",
        "date_finish": "2026-03-06T02:35:26+01:00",
        "operating_reset_at": 1772754326,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`recurring_deal`](#recurring-deal) | Root element of the response. Contains fields of the recurring deal template settings ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Type recurring_deal {#recurring-deal}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the record in the recurring deal settings table ||
|| **DEAL_ID**
[`integer`](../../../data-types.md) | Identifier of the deal for which the recurrence is set ||
|| **BASED_ID**
[`integer`](../../../data-types.md) | Identifier of the deal on which the template is based ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Active flag. Possible values:
- `Y` — active
- `N` — inactive ||
|| **NEXT_EXECUTION**
[`datetime`](../../../data-types.md) | Date and time of the next deal creation from the template ||
|| **LAST_EXECUTION**
[`datetime`](../../../data-types.md) | Date and time of the last deal creation from the template.

May be an empty string if there have been no executions yet ||
|| **COUNTER_REPEAT**
[`integer`](../../../data-types.md) | Number of deals created from the template.||
|| **START_DATE**
[`datetime`](../../../data-types.md) | Date to start calculating the next execution.

Returned in the format `YYYY-MM-DDTHH:MI:SS+TZD` ||
|| **CATEGORY_ID**
[`char`](../../../data-types.md) | Identifier of the funnel for created deals ||
|| **IS_LIMIT**
[`char`](../../../data-types.md) | Type of limitation on deal creation:
- `N` — no limitations
- `D` — by date (`LIMIT_DATE`)
- `T` — by quantity (`LIMIT_REPEAT`) ||
|| **LIMIT_REPEAT**
[`integer`](../../../data-types.md) | Maximum number of deals to be created. Used when `IS_LIMIT = T`||
|| **LIMIT_DATE**
[`datetime`](../../../data-types.md) | End date for deal generation. Used when `IS_LIMIT = D`.

Returned in the format `YYYY-MM-DDTHH:MI:SS+TZD` ||
|| **PARAMS**
[`object`](../../../data-types.md) | Recurrence parameters [(detailed description)](#params-fields) ||
|#

#### Fields of the PARAMS Object {#params-fields}

#|
|| **Name**
`type` | **Description** ||
|| **MODE**
[`string`](../../../data-types.md) | Recurrence mode:
- `single` — single
- `multiple` — multiple ||
|| **MULTIPLE_TYPE**
[`string`](../../../data-types.md) | Period type for `MODE = multiple`:
- `day`
- `week`
- `month`
- `year` ||
|| **MULTIPLE_INTERVAL**
[`integer`](../../../data-types.md) | Recurrence interval for `MODE = multiple` ||
|| **SINGLE_BEFORE_START_DATE_TYPE**
[`string`](../../../data-types.md) | Offset type for `MODE = single`:
- `day`
- `week`
- `month`
- `year`||
|| **SINGLE_BEFORE_START_DATE_VALUE**
[`integer`](../../../data-types.md) | Offset value for `MODE = single`.

For `MODE = multiple`, it usually returns `0` ||
|| **OFFSET_BEGINDATE_TYPE**
[`string`](../../../data-types.md) | Type of offset for the start date of the created deal:
- `day`
- `week`
- `month`
- `year` ||
|| **OFFSET_BEGINDATE_VALUE**
[`integer`](../../../data-types.md) | Offset value for the start date of the created deal ||
|| **OFFSET_CLOSEDATE_TYPE**
[`string`](../../../data-types.md) | Type of offset for the end date of the created deal:
- `day`
- `week`
- `month`
- `year` ||
|| **OFFSET_CLOSEDATE_VALUE**
[`integer`](../../../data-types.md) | Offset value for the end date of the created deal ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Recurring deal is not found."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `id is not defined or invalid` | The parameter `id` has been passed with an empty or incorrect value ||
|| `Recurring is not allowed` | Recurring deals are not available in Bitrix24 ||
|| `Recurring deal is not found` | The recurring deal template was not found ||
|| `Access denied` | Insufficient rights to read the recurring deal template ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-recurring-fields.md)
- [{#T}](./crm-deal-recurring-list.md)
- [{#T}](./crm-deal-recurring-add.md)
- [{#T}](./crm-deal-recurring-update.md)
- [{#T}](./crm-deal-recurring-delete.md)
- [{#T}](./crm-deal-recurring-expose.md)