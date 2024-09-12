# Add a setting for recurring invoice crm.invoice.recurring.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`Universal methods for invoices`](../../universal/invoice.md)

{% endnote %}

The method adds a new setting for a recurring invoice.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields**
[`array`](../../../data-types.md) | Field values for creating the recurring invoice setting.
The required field is `INVOICE_ID` [ID of the invoice that has the parameter `IS_RECURRING=Y`]. 

To find out the required format of the fields, execute the method [crm.invoice.recurring.fields](./crm-invoice-recurring-fields.md) and check the format of the received values for these fields ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"INVOICE_ID":"10","IS_LIMIT":"N","START_DATE":"'$(date -Iseconds --utc --date='TZ="America/New_York" +1 month')'","PARAMS":{"PERIOD":"day","IS_WORKING_ONLY":"N","INTERVAL":30,"DATE_PAY_BEFORE_OFFSET_TYPE":"month","DATE_PAY_BEFORE_OFFSET_VALUE":1}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.invoice.recurring.add    
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"INVOICE_ID":"10","IS_LIMIT":"N","START_DATE":"'$(date -Iseconds --utc --date='TZ="America/New_York" +1 month')'","PARAMS":{"PERIOD":"day","IS_WORKING_ONLY":"N","INTERVAL":30,"DATE_PAY_BEFORE_OFFSET_TYPE":"month","DATE_PAY_BEFORE_OFFSET_VALUE":1}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.invoice.recurring.add
    ```

- JS

    ```js
    var current = new Date();
    var nextMonth = new Date();
    nextMonth.setMonth(current.getMonth() + 1);
    var date2str = function(d)
    {
        return d.getFullYear() + '-' + paddatepart(1 + d.getMonth()) + '-' + paddatepart(d.getDate()) + 'T' + paddatepart(d.getHours()) + ':' + paddatepart(d.getMinutes()) + ':' + paddatepart(d.getSeconds()) + '+03:00';
    };
    var paddatepart = function(part)
    {
        return part >= 10 ? part.toString() : '0' + part.toString();
    };
    BX24.callMethod(
        "crm.invoice.recurring.add",
        {
            fields:
            {
                "INVOICE_ID": "10",
                "IS_LIMIT": "N",
                "START_DATE": date2str(nextMonth),
                "PARAMS": {
                    "PERIOD": "day",
                    "IS_WORKING_ONLY": "N",
                    "INTERVAL": 30,
                    "DATE_PAY_BEFORE_OFFSET_TYPE": "month",
                    "DATE_PAY_BEFORE_OFFSET_VALUE": 1,
                }
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("Recurring invoice settings added. Record ID - " + result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $current = new DateTime();
    $nextMonth = (new DateTime())->modify('+1 month');

    function date2str($d) {
        return $d->format('Y-m-d\TH:i:s+03:00');
    }

    $result = CRest::call(
        'crm.invoice.recurring.add',
        [
            'fields' => [
                'INVOICE_ID' => 10,
                'IS_LIMIT' => 'N',
                'START_DATE' => date2str($nextMonth),
                'PARAMS' => [
                    'PERIOD' => 'day',
                    'IS_WORKING_ONLY' => 'N',
                    'INTERVAL' => 30,
                    'DATE_PAY_BEFORE_OFFSET_TYPE' => 'month',
                    'DATE_PAY_BEFORE_OFFSET_VALUE' => 1,
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}