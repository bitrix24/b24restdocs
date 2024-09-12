# Update Deal Direction crm.dealcategory.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`crm.category.update`](../../universal/category/crm-category-update.md)

{% endnote %}

This method updates an existing direction.

## Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`integer`](../../../data-types.md)| Identifier of the direction ||
|| **fields**
[`array`](../../../data-types.md) | Field values for updating the deal direction.

To find out the required format of the fields, execute the method [`crm.dealcategory.fields`](./crm-deal-category-fields.md) and check the format of the returned values for these fields (except for fields marked with the attributes **isReadOnly** and **isImmutable**) ||
|#

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"1","fields":{"SORT":"100"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.dealcategory.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"1","fields":{"SORT":"100"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.dealcategory.update
    ```

- JS

    ```js
    var id = prompt("Enter ID");
    var sort = prompt("Enter sort order");
    sort = parseInt(sort);
    if(isNaN(sort) || sort < 0)
    {
        sort = 0;
    }

    BX24.callMethod(
        "crm.dealcategory.update",
        {
            id: id,
            fields: { "SORT": sort }
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

    $id = 1; // Replace 1 with the actual ID
    $sort = 100; // Replace 100 with the actual sort value

    $result = CRest::call(
        'crm.dealcategory.update',
        [
            'id' => $id,
            'fields' => ['SORT' => $sort]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}