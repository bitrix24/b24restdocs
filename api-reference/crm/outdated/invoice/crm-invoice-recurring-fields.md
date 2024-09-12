# Get the full template of the recurring invoice crm.invoice.recurring.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`Universal methods for invoices`](../../universal/invoice.md)

{% endnote %}

The method returns a list of fields for the recurring invoice template along with descriptions.

No parameters.

## Code Examples

{% include [Note about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.invoice.recurring.fields
   ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.invoice.recurring.fields
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.invoice.recurring.fields",
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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.invoice.recurring.fields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

### Returned Data

{% include [Note about required parameters](../../../../_includes/required.md) %}

#|
|| **Field** / **Type** | **Description** | **Note** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the record in the recurring invoice settings table | Read-only ||
|| **INVOICE_ID**
[`integer`](../../../data-types.md) | ID of the invoice template | Immutable ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Active flag. Values: Y/N | ||
|| **NEXT_EXECUTION**
[`date`](../../../data-types.md) | Date of the next creation of a new invoice from the template. Calculated by the system based on the specified parameters. If the value is empty, new invoices are not created | Read-only. ||
|| **LAST_EXECUTION**
[`date`](../../../data-types.md) | Date of the last creation of a new invoice from the template | Read-only ||
|| **COUNTER_REPEAT**
[`integer`](../../../data-types.md) | Number of invoices created from the template | Read-only ||
|| **START_DATE**
[`date`](../../../data-types.md) | Start date for calculating the date of the next creation of a new invoice | ||
|| **SEND_BILL**
[`char`](../../../data-types.md) | Send the invoice to the email associated with the payer. Values: Y/N | ||
|| **EMAIL_ID**
[`integer`](../../../data-types.md) | ID of the field containing the payer's email | ||
|| **IS_LIMIT**
[`char`](../../../data-types.md) | Are there any restrictions on creating new invoices | ||
|| **LIMIT_REPEAT**
[`integer`](../../../data-types.md) | Maximum number of invoices that can be created from this template | Considered if `IS_LIMIT` is equal to `T` ||
|| **LIMIT_DATE**
[`date`](../../../data-types.md) | Date until which invoices can be created from this template | Considered if `IS_LIMIT` is equal to `D` ||
|| **PARAMS**
[`unknown`](../../../data-types.md)
| Set of parameters for calculation - recurring_params: 
- **PERIOD** - repetition period:
    - day - day
    - week - week
    - month - month
    - year - year
- **TYPE** - type of repetition for month and year:
    - if PERIOD is month
        - 1 - calculation by the ordinal day number in the month
        - 2 - calculation by the day of the week numbers in the month
    - if PERIOD is year
        - 1 - calculation by the ordinal day number in the specified month
        - 2 - calculation by the day of the week numbers in the specified month
- **INTERVAL** - offset in calculation
- **IS_WORKING_ONLY** - only working days are considered (Y/N)
- **WEEKDAY** - full name of the day of the week (according to the formatting of the PHP method `date()`)
- **NUM_DAY_IN_MONTH** - ordinal date number in the month (PERIOD is month or year)
- **NUM_WEEKDAY_IN_MONTH**  - number of the weekday in the month (PERIOD is month or year)
- **FIELD_YEARLY_INTERVAL_MONTH_NAME** - number of the weekday in the month (PERIOD is month or year)
- **DATE_PAY_BEFORE_OFFSET_TYPE** - offset value for calculating the payment deadline, calculation is made from the moment of creating a new invoice from the template:
    - day - day
    - week - week
    - month - month
    - year - year
- **DATE_PAY_BEFORE_OFFSET_VALUE** - offset value for calculating the payment deadline, calculation is made from the moment of creating a new invoice from the template| ||
|#