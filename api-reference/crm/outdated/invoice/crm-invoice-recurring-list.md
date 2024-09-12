# Get a List of Recurring Invoice Settings by Filter crm.invoice.recurring.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`Universal methods for invoices`](../../universal/invoice.md)

{% endnote %}

The method returns a list of template settings for recurring invoices based on the filter.

When querying, use the mask "*" to select all fields (excluding custom and multiple fields).

## Method Parameters

See the description of [list methods](../../../how-to-call-rest-api/list-methods-pecularities.md).

## Code Examples

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"INVOICE_ID":"ASC"},"filter":{">COUNTER_REPEAT":5},"select":["ID","INVOICE_ID","NEXT_EXECUTION","LAST_EXECUTION","SEND_BILL","IS_LIMIT"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.invoice.recurring.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"INVOICE_ID":"ASC"},"filter":{">COUNTER_REPEAT":5},"select":["ID","INVOICE_ID","NEXT_EXECUTION","LAST_EXECUTION","SEND_BILL","IS_LIMIT"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.invoice.recurring.list
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.invoice.recurring.list",
        {
            order: { "INVOICE_ID": "ASC" },
            filter: { ">COUNTER_REPEAT": 5 },
            select: [ "ID", "INVOICE_ID ", "NEXT_EXECUTION", "LAST_EXECUTION", "SEND_BILL", "IS_LIMIT" ]
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.dir(result.data());
                if(result.more())
                    result.next();
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.invoice.recurring.list',
        [
            'order' => ['INVOICE_ID' => 'ASC'],
            'filter' => ['>COUNTER_REPEAT' => 5],
            'select' => ['ID', 'INVOICE_ID', 'NEXT_EXECUTION', 'LAST_EXECUTION', 'SEND_BILL', 'IS_LIMIT']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}