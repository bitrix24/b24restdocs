# Change Settings for Recurring Invoice crm.invoice.recurring.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

This method is deprecated. It is recommended to use [`Universal Methods for Invoices`](../../universal/invoice.md)

{% endnote %}

This method updates an existing setting for the recurring invoice template.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the recurring invoice template setting ||
|| **fields**
[`array`](../../../data-types.md) | Field values for updating the setting.

To find out the required format of the fields, execute the method [crm.invoice.recurring.fields](./crm-invoice-recurring-fields.md) and check the format of the returned values for these fields ||
|#

## Code Examples

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_recurring_invoice_id","fields":{"SEND_BILL":"Y","EMAIL_ID":136,"PARAMS":{"MODE":"month","TYPE":2,"INTERVAL":3,"WEEKDAY":"Monday","NUM_WEEKDAY_IN_MONTH":4,"DATE_PAY_BEFORE_OFFSET_TYPE":"day","DATE_PAY_BEFORE_OFFSET_VALUE":15}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.invoice.recurring.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_recurring_invoice_id","fields":{"SEND_BILL":"Y","EMAIL_ID":136,"PARAMS":{"MODE":"month","TYPE":2,"INTERVAL":3,"WEEKDAY":"Monday","NUM_WEEKDAY_IN_MONTH":4,"DATE_PAY_BEFORE_OFFSET_TYPE":"day","DATE_PAY_BEFORE_OFFSET_VALUE":15}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.invoice.recurring.update
    ```

- JS

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.invoice.recurring.update",
        {
            id: id,
            fields:
            {
                "SEND_BILL": "Y",
                "EMAIL_ID": 136,
                "PARAMS": {
                    "MODE": "month",
                    "TYPE": 2,
                    "INTERVAL": 3,
                    "WEEKDAY": "Monday",
                    "NUM_WEEKDAY_IN_MONTH": 4,
                    "DATE_PAY_BEFORE_OFFSET_TYPE": "day",
                    "DATE_PAY_BEFORE_OFFSET_VALUE": 15,
                }
            },
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $id = 'your_recurring_invoice_id'; // Replace 'your_recurring_invoice_id' with the actual recurring invoice ID

    $result = CRest::call(
        'crm.invoice.recurring.update',
        [
            'id' => $id,
            'fields' => [
                'SEND_BILL' => 'Y',
                'EMAIL_ID' => 136,
                'PARAMS' => [
                    'MODE' => 'month',
                    'TYPE' => 2,
                    'INTERVAL' => 3,
                    'WEEKDAY' => 'Monday',
                    'NUM_WEEKDAY_IN_MONTH' => 4,
                    'DATE_PAY_BEFORE_OFFSET_TYPE' => 'day',
                    'DATE_PAY_BEFORE_OFFSET_VALUE' => 15,
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}