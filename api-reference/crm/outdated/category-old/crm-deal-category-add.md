# Create a New Deal Category crm.dealcategory.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`crm.category.add`](../../universal/category/crm-category-add.md)

{% endnote %}

This method creates a new deal category.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields**
[`array`](../../../data-types.md) | Field values for creating a deal category.

To find out the required format for the fields, execute the method [`crm.dealcategory.fields`](./crm-deal-category-fields.md) and check the format of the returned field values.
||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"NAME":"New Category","SORT":"20"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.dealcategory.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"NAME":"New Category","SORT":"20"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.dealcategory.add
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.dealcategory.add",
        {
            fields:
            {
                "NAME": "New Category",
                "SORT": "20"
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("Category created with ID " + result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.dealcategory.add',
        [
            'fields' =>
            [
                'NAME' => 'New Category',
                'SORT' => '20'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}