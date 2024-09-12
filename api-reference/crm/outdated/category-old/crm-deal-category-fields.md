# Get Descriptions of Deal Category Fields crm.dealcategory.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`crm.category.fields`](../../universal/category/crm-category-fields.md)

{% endnote %}

The method returns the description of deal category fields.

No parameters.

## Code Examples

{% include [Note about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.dealcategory.fields
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.dealcategory.fields
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.dealcategory.fields",
        {},
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

    $result = CRest::call(
        'crm.dealcategory.fields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

### Returned Data

{% include [Note about required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CREATED_DATE** 
[`datetime`](../../../data-types.md) | Creation date  ||
|| **ID** 
[`integer`](../../../data-types.md)| Deal category identifier ||
|| **IS_LOCKED**
[`char`](../../../data-types.md) | Is locked  ||
|| **NAME***
[`string`](../../../data-types.md)| Category name  ||
|| **SORT** 
[`integer`](../../../data-types.md) | Sorting   ||
|#