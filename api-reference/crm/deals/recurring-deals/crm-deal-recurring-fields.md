# Get the List of Fields for the Recurring Deal Template crm.deal.recurring.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "read" access permission for deals

The method `crm.deal.recurring.fields` returns a description of the fields for the recurring deal template.

## Method Parameters

No parameters

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.recurring.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.recurring.fields
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.deal.recurring.fields',
    		{}
    	);
    
    	const result = response.getData().result;
    	console.dir(result);
    }
    catch(error)
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
                'crm.deal.recurring.fields',
                []
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
        echo 'Error fetching recurring deal fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'crm.deal.recurring.fields',
        {},
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.recurring.fields',
        []
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
        "ID": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "id"
        },
        "DEAL_ID": {
            "type": "integer",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "id of the recurring deal"
        },
        "BASED_ID": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Created based on"
        },
        "ACTIVE": {
            "type": "char",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Active"
        },
        "NEXT_EXECUTION": {
            "type": "datetime",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Next execution date"
        },
        "LAST_EXECUTION": {
            "type": "datetime",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Last execution date"
        },
        "COUNTER_REPEAT": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Number of executions"
        },
        "START_DATE": {
            "type": "date",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Start date for calculation"
        },
        "CATEGORY_ID": {
            "type": "char",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Category of the new deal"
        },
        "IS_LIMIT": {
            "type": "char",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Type of execution limits"
        },
        "LIMIT_REPEAT": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Limit on the number of executions"
        },
        "LIMIT_DATE": {
            "type": "date",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Limit by date"
        },
        "PARAMS": {
            "type": "recurring_params",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "definition": {
                "MODE": {
                    "type": "string",
                    "isRequired": false,
                    "isReadOnly": false,
                    "isImmutable": false,
                    "isMultiple": false,
                    "isDynamic": false,
                    "title": "Repetition mode"
                },
                "SINGLE_BEFORE_START_DATE_VALUE": {
                    "type": "integer",
                    "isRequired": false,
                    "isReadOnly": false,
                    "isImmutable": false,
                    "isMultiple": false,
                    "isDynamic": false,
                    "title": "Offset before the calculation date"
                },
                "SINGLE_BEFORE_START_DATE_TYPE": {
                    "type": "string",
                    "isRequired": false,
                    "isReadOnly": false,
                    "isImmutable": false,
                    "isMultiple": false,
                    "isDynamic": false,
                    "title": "Type of offset before the calculation date"
                },
                "MULTIPLE_TYPE": {
                    "type": "string",
                    "isRequired": false,
                    "isReadOnly": false,
                    "isImmutable": false,
                    "isMultiple": false,
                    "isDynamic": false,
                    "title": "Type of repetition"
                },
                "MULTIPLE_INTERVAL": {
                    "type": "integer",
                    "isRequired": false,
                    "isReadOnly": false,
                    "isImmutable": false,
                    "isMultiple": false,
                    "isDynamic": false,
                    "title": "Repetition interval"
                },
                "OFFSET_BEGINDATE_TYPE": {
                    "type": "string",
                    "isRequired": false,
                    "isReadOnly": false,
                    "isImmutable": false,
                    "isMultiple": false,
                    "isDynamic": false,
                    "title": "Type of offset for the start date in the new deal"
                },
                "OFFSET_BEGINDATE_VALUE": {
                    "type": "integer",
                    "isRequired": false,
                    "isReadOnly": false,
                    "isImmutable": false,
                    "isMultiple": false,
                    "isDynamic": false,
                    "title": "Offset for the start date in the new deal"
                },
                "OFFSET_CLOSEDATE_TYPE": {
                    "type": "string",
                    "isRequired": false,
                    "isReadOnly": false,
                    "isImmutable": false,
                    "isMultiple": false,
                    "isDynamic": false,
                    "title": "Type of offset for the completion date in the new deal"
                },
                "OFFSET_CLOSEDATE_VALUE": {
                    "type": "integer",
                    "isRequired": false,
                    "isReadOnly": false,
                    "isImmutable": false,
                    "isMultiple": false,
                    "isDynamic": false,
                    "title": "Offset for the completion date in the new deal"
                }
            },
            "title": "Parameters for calculating the next execution date"
        }
    },
    "time": {
        "start": 1772690452,
        "finish": 1772690452.454308,
        "duration": 0.45430803298950195,
        "processing": 0,
        "date_start": "2026-03-05T09:00:52+01:00",
        "date_finish": "2026-03-05T09:00:52+01:00",
        "operating_reset_at": 1772691052,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response. Contains a description of the [fields of the template](#all-fields) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Overview of the Recurring Deal Template Fields {#all-fields}

#|
|| **Field** `type` | **Description** | **Note** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the record in the recurring deal settings table | Read-only ||
|| **DEAL_ID**
[`integer`](../../../data-types.md) | id of the recurring deal | Immutable ||
|| **BASED_ID**
[`integer`](../../../data-types.md) | id of the deal on which the template was created | Read-only ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Active flag | Values: `Y` / `N` ||
|| **NEXT_EXECUTION**
[`datetime`](../../../data-types.md) | Date and time of the next deal creation from the template | Read-only ||
|| **LAST_EXECUTION**
[`datetime`](../../../data-types.md) | Date and time of the last deal creation from the template | Read-only ||
|| **COUNTER_REPEAT**
[`integer`](../../../data-types.md) | Number of deals created from the template | Read-only ||
|| **START_DATE**
[`date`](../../../data-types.md) | Date to start calculating the next launch | If not specified, calculation starts from the current date ||
|| **CATEGORY_ID**
[`char`](../../../data-types.md) | id of the funnel for created deals | ||
|| **IS_LIMIT**
[`char`](../../../data-types.md) | Type of limit on deal creation | `N` — no limits, `D` — by date, `T` — by quantity ||
|| **LIMIT_REPEAT**
[`integer`](../../../data-types.md) | Maximum number of deals to be created | Used when `IS_LIMIT = T` ||
|| **LIMIT_DATE**
[`date`](../../../data-types.md) | End date for deal generation | Used when `IS_LIMIT = D` ||
|| **PARAMS**
[`recurring_params`](../../../data-types.md) | Parameters for calculating the next execution date | Structure of fields — [below](#params-fields) ||
|#

#### Fields of the PARAMS Object {#params-fields}

#|
|| **Field** `type` | **Description** | **Note** ||
|| **MODE**
[`string`](../../../data-types.md) | Repetition mode | `single` — single, `multiple` — multiple ||
|| **MULTIPLE_TYPE**
[`string`](../../../data-types.md) | Type of period for `MODE = multiple` | `day`, `week`, `month`, `year` ||
|| **MULTIPLE_INTERVAL**
[`integer`](../../../data-types.md) | Repetition interval for `MODE = multiple` | ||
|| **SINGLE_BEFORE_START_DATE_TYPE**
[`string`](../../../data-types.md) | Type of offset for `MODE = single` | `day`, `week`, `month`, `year` ||
|| **SINGLE_BEFORE_START_DATE_VALUE**
[`integer`](../../../data-types.md) | Value of the offset for `MODE = single` | ||
|| **OFFSET_BEGINDATE_TYPE**
[`string`](../../../data-types.md) | Type of offset for the start date of the created deal | `day`, `week`, `month`, `year` ||
|| **OFFSET_BEGINDATE_VALUE**
[`integer`](../../../data-types.md) | Value of the offset for the start date of the created deal | ||
|| **OFFSET_CLOSEDATE_TYPE**
[`string`](../../../data-types.md) | Type of offset for the completion date of the created deal | `day`, `week`, `month`, `year` ||
|| **OFFSET_CLOSEDATE_VALUE**
[`integer`](../../../data-types.md) | Value of the offset for the completion date of the created deal | ||
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
|| `Access denied` | Insufficient permissions to access CRM ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-recurring-add.md)
- [{#T}](./crm-deal-recurring-get.md)
- [{#T}](./crm-deal-recurring-list.md)
- [{#T}](./crm-deal-recurring-update.md)
- [{#T}](./crm-deal-recurring-delete.md)
- [{#T}](./crm-deal-recurring-expose.md)