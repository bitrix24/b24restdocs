# Get a list of payer types crm.persontype.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`Universal methods for invoices`](../../universal/invoice.md)

{% endnote %}

The method returns a list of payer types.

{% note info %}

For payment systems used in CRM (for invoices, deals), payer types should be retrieved using the `crm.persontype.list` method. If a payment system is being created for orders, then the method [`sale.persontype.list`](../../../sale/person-type/sale-person-type-list.md) should be used.

{% endnote %}

#|
|| **Name**
`type` | **Description** ||
|| **order** | Sorting fields ||
|| **filter** | Filtering fields ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"ASC"},"filter":{"NAME":"CRM_COMPANY"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.persontype.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"ASC"},"filter":{"NAME":"CRM_COMPANY"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.persontype.list
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.persontype.list", {
            order: {"ID": "ASC"},
            filter: {
                "NAME": "CRM_COMPANY",
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
        'crm.persontype.list',
        [
            'order' => ['ID' => 'ASC'],
            'filter' => ['NAME' => 'CRM_COMPANY']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}