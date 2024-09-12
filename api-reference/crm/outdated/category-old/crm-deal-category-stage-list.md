# Get a List of Deal Stages for the crm.dealcategory.stage.list Endpoint

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use the funnel methods [`crm.category.*`](../../universal/category/index.md)

{% endnote %}

The method returns a list of deal stages for the category by its identifier. It is equivalent to calling the method [`crm.status.list`](../../status/crm-status-list.md) with the `ENTITY_ID` parameter set to the result of the call to [`crm.dealcategory.status`](crm-deal-category-status.md).

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`integer`](../../../data-types.md)| Identifier of the category. If you specify `id = 0` or do not [specify anything](*quotes), it will return the statuses of the "default" category. If you specify `id > 0` for a [non-existent category](*id), it returns nothing ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"1"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.dealcategory.stage.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"1","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.dealcategory.stage.list
    ```

- JS

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.dealcategory.stage.list",
        { id: id },
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

    $id = 1; // Replace 1 with the actual ID

    $result = CRest::call(
        'crm.dealcategory.stage.list',
        [
            'id' => $id
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}


[*quotes]: Empty quotes or not passing the parameter at all

[*id]: For example, specifying id = 10, but there is no category with id=10 in the system