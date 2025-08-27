# Get the list of fields for the recurring deal template crm.deal.recurring.fields

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing (in other languages)
- success response is missing
- error response is missing
- links to pages that have not yet been created are not specified

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.recurring.fields` returns a list of fields for configuring the recurring deal template along with descriptions.

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.deal.recurring.fields',
    		{}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.dir(result);
    	}
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
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching recurring deal fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.deal.recurring.fields",
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

{% endlist %}

{% include [Examples note](../../../../_includes/examples.md) %}

## Returned fields

#|
|| **Field** | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the record in the settings table for the recurring deal. Read-only. ||
|| **DEAL_ID**
[`integer`](../../../data-types.md) | ID of the deal template. Immutable. ||
|| **BASED_ID**
[`integer`](../../../data-types.md) | ID of the deal on which the template was based. Immutable. ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Active flag. Values: Y/N. ||
|| **NEXT_EXECUTION**
[`date`](../../../data-types.md) | Date of the next creation of a new deal from the template. Calculated by the system based on the specified parameters. If the value is empty, new deals are not created. Read-only. ||
|| **LAST_EXECUTION**
[`date`](../../../data-types.md) | Date of the last creation of a new deal from the template. Read-only. ||
|| **COUNTER_REPEAT**
[`integer`](../../../data-types.md) | Number of deals created from the template. Read-only. ||
|| **START_DATE**
[`date`](../../../data-types.md) | Start date for calculating the date of the next creation of a new deal. If the value is empty, it is calculated from the current date. ||
|| **CATEGORY_ID**
[`char`](../../../data-types.md) | Category that will be assigned to the deal created from the template. ||
|| **IS_LIMIT**
[`char`](../../../data-types.md) | Are there restrictions on creating new deals? Values: N - no restrictions, D - date restriction set, T - limit on the number of new deals set. ||
|| **LIMIT_REPEAT**
[`integer`](../../../data-types.md) | Maximum number of deals that can be created from this template. Considered if `IS_LIMIT` is T. ||
|| **LIMIT_DATE**
[`date`](../../../data-types.md) | Date until which deals can be created from this template. Considered if `IS_LIMIT` is D. ||
|| **PARAMS** | Set of parameters for calculation - `recurring_params`:

- **MODE** - repetition mode:
    - single - single (one deal will be created from the template, offset is calculated to the value of START_DATE);
    - multiple - multiple

- **MULTIPLE_TYPE** - type of repetition in multiple mode [MODE is multiple]:
    - day - day
    - week - week
    - month - month
    - year - year

- **MULTIPLE_INTERVAL** - offset value [MODE is multiple]

- **SINGLE_BEFORE_START_DATE_TYPE** - type of offset before the start date [MODE is single]:
    - day - day
    - week - week
    - month - month
    - year - year

- **SINGLE_BEFORE_START_DATE_VALUE** - offset value before the start date, if not set - no offset [MODE is single]

- **OFFSET_BEGINDATE_TYPE** - type of offset for calculating the "deal start date" field, calculation is made from the moment of creating a new deal from the template:
    - day - day
    - week - week
    - month - month
    - year - year

- **OFFSET_BEGINDATE_VALUE** - offset value for calculating the "deal start date" field, calculation is made from the moment of creating a new deal from the template

- **OFFSET_CLOSEDATE_TYPE** - offset value for calculating the "deal completion date" field, calculation is made from the moment of creating a new deal from the template:
    - day - day
    - week - week
    - month - month
    - year - year

- **OFFSET_CLOSEDATE_VALUE** - offset value for calculating the "deal completion date" field, calculation is made from the moment of creating a new deal from the template ||
|#

{% note tip "Related methods and topics" %}

[{#T}](./crm-deal-recurring-add.md)

{% endnote %}