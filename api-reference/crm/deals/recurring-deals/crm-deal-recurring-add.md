# Create a Template for Recurring Deal crm.deal.recurring.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "add" and "edit" permissions for deals

The method `crm.deal.recurring.add` creates a template for a recurring deal based on an existing deal.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields^*^**
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
- `field_n` — name of the setting field
- `value_n` — value of the setting field

The list of available fields is described [below](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **DEAL_ID**
[`integer`](../../../data-types.md) | Identifier of the deal based on which the recurring deal template is created.

The deal identifier can be obtained using the methods [crm.deal.list](../crm-deal-list.md) or [crm.deal.add](../crm-deal-add.md) ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Activity flag of the setting. Possible values:
- `Y` — active
- `N` — inactive
||
|| **CATEGORY_ID**
[`char`](../../../data-types.md) | ID of the funnel for new deals created from the template.

The list of available funnels can be obtained using the method [crm.category.list](../../universal/category/crm-category-list.md) by passing `entityTypeId = 2` ||
|| **IS_LIMIT**
[`char`](../../../data-types.md) | Limit on creating new deals:
`N` — no limits
`D` — limit by date (`LIMIT_DATE`)
`T` — limit by quantity (`LIMIT_REPEAT`)

If any other value is passed, it will be converted to `N` ||
|| **LIMIT_REPEAT**
[`integer`](../../../data-types.md) | Maximum number of deals that can be created from the template. Used when `IS_LIMIT = T` ||
|| **LIMIT_DATE**
[`date`](../../../data-types.md) | End date for generating deals from the template in the format `YYYY-MM-DD`. Used when `IS_LIMIT = D` ||
|| **START_DATE**
[`date`](../../../data-types.md) | Start date for calculating the next run in the format `YYYY-MM-DD` ||
|| **PARAMS**
[`object`](../../../data-types.md) | Frequency parameters [(detailed description)](#params) ||
|#

### Parameter PARAMS {#params}

#|
|| **Name**
`type` | **Description** ||
|| **MODE**
[`string`](../../../data-types.md) | Repetition mode:
`single` — single
`multiple` — multiple ||
|| **MULTIPLE_TYPE**
[`string`](../../../data-types.md) | Type of period for `MODE = multiple`:
`day`, `week`, `month`, `year` ||
|| **MULTIPLE_INTERVAL**
[`integer`](../../../data-types.md) | Repetition interval for `MODE = multiple` ||
|| **SINGLE_BEFORE_START_DATE_TYPE**
[`string`](../../../data-types.md) | Offset type for `MODE = single`:
`day`, `week`, `month`, `year` ||
|| **SINGLE_BEFORE_START_DATE_VALUE**
[`integer`](../../../data-types.md) | Offset value for `MODE = single` ||
|| **OFFSET_BEGINDATE_TYPE**
[`string`](../../../data-types.md) | Offset type for the start date of the created deal:
`day`, `week`, `month`, `year` ||
|| **OFFSET_BEGINDATE_VALUE**
[`integer`](../../../data-types.md) | Offset value for the start date of the created deal ||
|| **OFFSET_CLOSEDATE_TYPE**
[`string`](../../../data-types.md) | Offset type for the end date of the created deal:
`day`, `week`, `month`, `year` ||
|| **OFFSET_CLOSEDATE_VALUE**
[`integer`](../../../data-types.md) | Offset value for the end date of the created deal ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"DEAL_ID":45,"CATEGORY_ID":"1","IS_LIMIT":"D","LIMIT_DATE":"2027-03-04","START_DATE":"2026-04-04","PARAMS":{"MODE":"multiple","MULTIPLE_TYPE":"month","MULTIPLE_INTERVAL":1,"OFFSET_BEGINDATE_TYPE":"day","OFFSET_BEGINDATE_VALUE":1,"OFFSET_CLOSEDATE_TYPE":"month","OFFSET_CLOSEDATE_VALUE":2}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.recurring.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"DEAL_ID":45,"CATEGORY_ID":"1","IS_LIMIT":"D","LIMIT_DATE":"2027-03-04","START_DATE":"2026-04-04","PARAMS":{"MODE":"multiple","MULTIPLE_TYPE":"month","MULTIPLE_INTERVAL":1,"OFFSET_BEGINDATE_TYPE":"day","OFFSET_BEGINDATE_VALUE":1,"OFFSET_CLOSEDATE_TYPE":"month","OFFSET_CLOSEDATE_VALUE":2}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.recurring.add
    ```

- JS

    ```js
    try
    {
    	const current = new Date();
    	const nextMonth = new Date();
    	const nextYear = new Date();
    	nextMonth.setMonth(current.getMonth() + 1);
    	nextYear.setFullYear(current.getFullYear() + 1);
    
    	const paddatepart = (part) => (part >= 10 ? part.toString() : `0${part}`);
    	const date2str = (d) =>
    		`${d.getFullYear()}-${paddatepart(1 + d.getMonth())}-${paddatepart(d.getDate())}`;
    
    	const response = await $b24.callMethod(
    		'crm.deal.recurring.add',
    		{
    			fields: {
    				DEAL_ID: 45,
    				CATEGORY_ID: '1',
    				IS_LIMIT: 'D',
    				LIMIT_DATE: date2str(nextYear),
    				START_DATE: date2str(nextMonth),
    				PARAMS: {
    					MODE: 'multiple',
    					MULTIPLE_TYPE: 'month',
    					MULTIPLE_INTERVAL: 1,
    					OFFSET_BEGINDATE_TYPE: 'day',
    					OFFSET_BEGINDATE_VALUE: 1,
    					OFFSET_CLOSEDATE_TYPE: 'month',
    					OFFSET_CLOSEDATE_VALUE: 2,
    				}
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info('Recurring deal setting ID:', result);
    }
    catch(error)
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $current = new DateTime();
        $nextMonth = (clone $current)->modify('+1 month');
        $nextYear = (clone $current)->modify('+1 year');
    
        $response = $b24Service
            ->core
            ->call(
                'crm.deal.recurring.add',
                [
                    'fields' => [
                        'DEAL_ID'     => 45,
                        'CATEGORY_ID' => '1',
                        'IS_LIMIT'    => 'D',
                        'LIMIT_DATE'  => $nextYear->format('Y-m-d'),
                        'START_DATE'  => $nextMonth->format('Y-m-d'),
                        'PARAMS'      => [
                            'MODE'                   => 'multiple',
                            'MULTIPLE_TYPE'          => 'month',
                            'MULTIPLE_INTERVAL'      => 1,
                            'OFFSET_BEGINDATE_TYPE'  => 'day',
                            'OFFSET_BEGINDATE_VALUE' => 1,
                            'OFFSET_CLOSEDATE_TYPE'  => 'month',
                            'OFFSET_CLOSEDATE_VALUE' => 2,
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: Recurring deal setting ID - ' . $result->data();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding recurring deal settings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.deal.recurring.add',
        {
            fields: {
                DEAL_ID: 45,
                CATEGORY_ID: '1',
                IS_LIMIT: 'D',
                LIMIT_DATE: '2027-03-04',
                START_DATE: '2026-04-04',
                PARAMS: {
                    MODE: 'multiple',
                    MULTIPLE_TYPE: 'month',
                    MULTIPLE_INTERVAL: 1,
                    OFFSET_BEGINDATE_TYPE: 'day',
                    OFFSET_BEGINDATE_VALUE: 1,
                    OFFSET_CLOSEDATE_TYPE: 'month',
                    OFFSET_CLOSEDATE_VALUE: 2
                }
            }
        },
        function(result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.info('Recurring deal setting ID:', result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.recurring.add',
        [
            'fields' => [
                'DEAL_ID' => 45,
                'CATEGORY_ID' => '1',
                'IS_LIMIT' => 'D',
                'LIMIT_DATE' => '2027-03-04',
                'START_DATE' => '2026-04-04',
                'PARAMS' => [
                    'MODE' => 'multiple',
                    'MULTIPLE_TYPE' => 'month',
                    'MULTIPLE_INTERVAL' => 1,
                    'OFFSET_BEGINDATE_TYPE' => 'day',
                    'OFFSET_BEGINDATE_VALUE' => 1,
                    'OFFSET_CLOSEDATE_TYPE' => 'month',
                    'OFFSET_CLOSEDATE_VALUE' => 2
                ]
            ]
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
    "result": 15,
    "time": {
        "start": 1772664456,
        "finish": 1772664457.379632,
        "duration": 1.3796319961547852,
        "processing": 1,
        "date_start": "2026-03-05T01:47:36+01:00",
        "date_finish": "2026-03-05T01:47:37+01:00",
        "operating_reset_at": 1772665056,
        "operating": 0.7352650165557861
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Identifier of the created recurring deal template setting ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Deal already has recurring settings."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `Parameter 'fields' must be array` | The `fields` parameter is not provided or is not in object format ||
|| `Parameter 'params' must be array` | The `params` parameter is not provided in object format ||
|| `Deal id is empty` | An empty or incorrect value is passed in `fields.DEAL_ID` ||
|| `Access denied` | Insufficient permissions to modify the original deal or create deals ||
|| `Deal already has recurring settings` | A recurring setting already exists for the provided deal ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-recurring-fields.md)
- [{#T}](./crm-deal-recurring-get.md)
- [{#T}](./crm-deal-recurring-list.md)
- [{#T}](./crm-deal-recurring-update.md)
- [{#T}](./crm-deal-recurring-delete.md)
- [{#T}](./crm-deal-recurring-expose.md)