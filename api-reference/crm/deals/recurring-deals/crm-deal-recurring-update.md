# Update the Recurring Deal Template Settings `crm.deal.recurring.update`

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "read" and "modify" access permissions for deals

The method `crm.deal.recurring.update` updates an existing recurring deal template setting.

## Method Parameters

{% include [Parameter Notes](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id^*^**
[`integer`](../../../data-types.md) | The identifier of the recurring deal template setting.

The identifier can be obtained using the methods [crm.deal.recurring.list](./crm-deal-recurring-list.md) or [crm.deal.recurring.add](./crm-deal-recurring-add.md) ||
|| **fields**
[`object`](../../../data-types.md) | An object in the following format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

where:
- `field_n` — the name of the setting field
- `value_n` — the new value for the setting field

The list of available fields is described [below](#fields) ||
|#

### Fields Parameter {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **ACTIVE**
[`char`](../../../data-types.md) | The active flag for the setting. Possible values:
- `Y` — active
- `N` — inactive ||
|| **CATEGORY_ID**
[`char`](../../../data-types.md) | The ID of the funnel for new deals created from the template.

The list of available funnels can be obtained using the method [crm.category.list](../../universal/category/crm-category-list.md) by passing `entityTypeId = 2` ||
|| **IS_LIMIT**
[`char`](../../../data-types.md) | Limit on creating new deals:
- `N` — no limits
- `D` — date limit (`LIMIT_DATE`)
- `T` — quantity limit (`LIMIT_REPEAT`) ||
|| **LIMIT_REPEAT**
[`integer`](../../../data-types.md) | The maximum number of deals that can be created from the template. Used when `IS_LIMIT = T` ||
|| **LIMIT_DATE**
[`date`](../../../data-types.md) | The end date for generating deals from the template in the format `YYYY-MM-DD`. Used when `IS_LIMIT = D` ||
|| **START_DATE**
[`date`](../../../data-types.md) | The start date for calculating the next run in the format `YYYY-MM-DD` ||
|| **PARAMS**
[`object`](../../../data-types.md) | Recurrence parameters [(detailed description)](#params-fields) ||
|#

### PARAMS Parameter {#params-fields}

#|
|| **Name**
`type` | **Description** ||
|| **MODE**
[`string`](../../../data-types.md) | The recurrence mode:
- `single` — single
- `multiple` — multiple ||
|| **MULTIPLE_TYPE**
[`string`](../../../data-types.md) | The period type for `MODE = multiple`:
- `day`
- `week`
- `month`
- `year` ||
|| **MULTIPLE_INTERVAL**
[`integer`](../../../data-types.md) | The recurrence interval for `MODE = multiple` ||
|| **SINGLE_BEFORE_START_DATE_TYPE**
[`string`](../../../data-types.md) | The offset type for `MODE = single`:
- `day`
- `week`
- `month`
- `year` ||
|| **SINGLE_BEFORE_START_DATE_VALUE**
[`integer`](../../../data-types.md) | The offset value for `MODE = single` ||
|| **OFFSET_BEGINDATE_TYPE**
[`string`](../../../data-types.md) | The offset type for the start date of the created deal:
- `day`
- `week`
- `month`
- `year` ||
|| **OFFSET_BEGINDATE_VALUE**
[`integer`](../../../data-types.md) | The offset value for the start date of the created deal ||
|| **OFFSET_CLOSEDATE_TYPE**
[`string`](../../../data-types.md) | The offset type for the completion date of the created deal:
- `day`
- `week`
- `month`
- `year` ||
|| **OFFSET_CLOSEDATE_VALUE**
[`integer`](../../../data-types.md) | The offset value for the completion date of the created deal ||
|#

## Code Examples

{% include [Example Notes](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15,"fields":{"ACTIVE":"Y","CATEGORY_ID":"2","IS_LIMIT":"D","LIMIT_DATE":"2027-03-05","START_DATE":"2026-04-05","PARAMS":{"MODE":"multiple","MULTIPLE_TYPE":"month","MULTIPLE_INTERVAL":1,"OFFSET_BEGINDATE_TYPE":"day","OFFSET_BEGINDATE_VALUE":1,"OFFSET_CLOSEDATE_TYPE":"month","OFFSET_CLOSEDATE_VALUE":2}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.recurring.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15,"fields":{"ACTIVE":"Y","CATEGORY_ID":"2","IS_LIMIT":"D","LIMIT_DATE":"2027-03-05","START_DATE":"2026-04-05","PARAMS":{"MODE":"multiple","MULTIPLE_TYPE":"month","MULTIPLE_INTERVAL":1,"OFFSET_BEGINDATE_TYPE":"day","OFFSET_BEGINDATE_VALUE":1,"OFFSET_CLOSEDATE_TYPE":"month","OFFSET_CLOSEDATE_VALUE":2}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.recurring.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.deal.recurring.update',
    		{
    			id: 15,
    			fields: {
    				ACTIVE: 'Y',
    				CATEGORY_ID: '2',
    				IS_LIMIT: 'D',
    				LIMIT_DATE: '2027-03-05',
    				START_DATE: '2026-04-05',
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
    	console.info('Template updated:', result);
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
                'crm.deal.recurring.update',
                [
                    'id' => 15,
                    'fields' => [
                        'ACTIVE'      => 'Y',
                        'CATEGORY_ID' => '2',
                        'IS_LIMIT'    => 'D',
                        'LIMIT_DATE'  => '2027-03-05',
                        'START_DATE'  => '2026-04-05',
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
            echo 'Success: ' . var_export($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating recurring deal settings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.deal.recurring.update',
        {
            id: 15,
            fields: {
                ACTIVE: 'Y',
                CATEGORY_ID: '2',
                IS_LIMIT: 'D',
                LIMIT_DATE: '2027-03-05',
                START_DATE: '2026-04-05',
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
                console.info('Template updated:', result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.recurring.update',
        [
            'id' => 15,
            'fields' => [
                'ACTIVE' => 'Y',
                'CATEGORY_ID' => '2',
                'IS_LIMIT' => 'D',
                'LIMIT_DATE' => '2027-03-05',
                'START_DATE' => '2026-04-05',
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
    "result": true,
    "time": {
        "start": 1772731825,
        "finish": 1772731825.579511,
        "duration": 0.5795109272003174,
        "processing": 0,
        "date_start": "2026-03-05T20:30:25+02:00",
        "date_finish": "2026-03-05T20:30:25+02:00",
        "operating_reset_at": 1772732425,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | The root element of the response. Returns `true` for a successful update ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Recurring deal is not found."
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `id is not defined or invalid` | The parameter `id` has been passed with an empty or invalid value ||
|| `Parameter 'fields' must be array` | The parameter `fields` is missing or not in object format ||
|| `Recurring is not allowed` | Recurring deals are not available in Bitrix24 ||
|| `Recurring deal is not found` | The recurring deal template was not found ||
|| `Access denied` | Insufficient permissions to read or modify the associated deal ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-recurring-fields.md)
- [{#T}](./crm-deal-recurring-get.md)
- [{#T}](./crm-deal-recurring-list.md)
- [{#T}](./crm-deal-recurring-add.md)
- [{#T}](./crm-deal-recurring-delete.md)
- [{#T}](./crm-deal-recurring-expose.md)