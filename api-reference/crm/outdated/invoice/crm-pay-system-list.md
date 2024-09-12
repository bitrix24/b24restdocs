# Get a list of payment methods crm.paysystem.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`Universal methods for invoices`](../../universal/invoice.md)

{% endnote %}

The method returns a list of payment methods applicable to estimates or invoices.

#|
|| **Name**
`type` | **Description** ||
|| **order** | Sorting fields ||
|| **filter** | Filter fields ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

The example outputs data to the console. If you need to output data differently, implement your own data handling for the results returned by `result.data()` and `result.error()`.

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"SORT":"ASC"},"filter":{"%NAME":"Estimate"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.paysystem.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"SORT":"ASC"},"filter":{"%NAME":"Estimate"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.paysystem.list
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.paysystem.list", {
            order: {"SORT": "ASC"},
            filter: {
                "%NAME": "Estimate",
            }
        },
        function (result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.dir(result.data());
                if (result.more())
                    result.next();
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.paysystem.list',
        [
            'order' => ['SORT' => 'ASC'],
            'filter' => ['%NAME' => 'Estimate']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}