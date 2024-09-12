# Get the list of measurement units crm.measure.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns a list of measurement units.

See the description of [list methods](../../../how-to-call-rest-api/list-methods-pecularities.md).

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"ASC"},"filter":{"IS_DEFAULT":"Y"},"select":["ID","CODE","STAGE_ID","SYMBOL_INTL","SYMBOL_RUS"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.measure.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"ASC"},"filter":{"IS_DEFAULT":"Y"},"select":["ID","CODE","STAGE_ID","SYMBOL_INTL","SYMBOL_RUS"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.measure.list
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.measure.list",
        {
            order: {"ID": "ASC"},
            filter: {"IS_DEFAULT": "Y"},
            select: ["ID", "CODE", "STAGE_ID", "SYMBOL_INTL", "SYMBOL_RUS"]
        },
        function (result)
        {
            if (result.error())
                console.error(result.error());
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
        'crm.measure.list',
        [
            'order' => ['ID' => 'ASC'],
            'filter' => ['IS_DEFAULT' => 'Y'],
            'select' => ['ID', 'CODE', 'STAGE_ID', 'SYMBOL_INTL', 'SYMBOL_RUS']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue Learning

- [{#T}](./crm-measure-add.md)
- [{#T}](./crm-measure-update.md)
- [{#T}](./crm-measure-get.md)
- [{#T}](./crm-measure-delete.md)
- [{#T}](./crm-measure-fields.md)