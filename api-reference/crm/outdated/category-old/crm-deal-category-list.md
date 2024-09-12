# Filter Deal Categories crm.dealcategory.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`crm.category.list`](../../universal/category/crm-category-list.md)

{% endnote %}

The method returns a list of deal categories based on the filter. It is an implementation of list methods for deal categories.

## Method Parameters

See the description of [list methods](../../../how-to-call-rest-api/list-methods-pecularities.md)

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"SORT":"ASC"},"filter":{"IS_LOCKED":"N"},"select":["ID","NAME","SORT"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.dealcategory.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"SORT":"ASC"},"filter":{"IS_LOCKED":"N"},"select":["ID","NAME","SORT"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.dealcategory.list
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.dealcategory.list",
        {
            order: { "SORT": "ASC" },
            filter: { "IS_LOCKED": "N" }, //Y - all categories, N - all categories except deleted. Deleted categories are not permanently removed from the database but only blocked.
            select: [ "ID", "NAME", "SORT" ]
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
        'crm.dealcategory.list',
        [
            'order' => ['SORT' => 'ASC'],
            'filter' => ['IS_LOCKED' => 'N'],
            'select' => ['ID', 'NAME', 'SORT']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}