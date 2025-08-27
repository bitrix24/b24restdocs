# Get descriptions of fields for deal categories crm.dealcategory.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`crm.category.fields`](../../universal/category/crm-category-fields.md)

{% endnote %}

The method returns descriptions of fields for deal categories.

No parameters.

## Code Examples

{% include [Footnote about examples](../../../../_includes/examples.md) %}

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
    try
    {
    	const response = await $b24.callMethod(
    		"crm.dealcategory.fields",
    		{}
    	);
    	
    	const result = response.getData().result;
    	console.dir(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.dealcategory.fields',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching deal category fields: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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

{% include [Footnote about required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CREATED_DATE** 
[`datetime`](../../../data-types.md) | Creation date  ||
|| **ID** 
[`integer`](../../../data-types.md)| Identifier of the deal category ||
|| **IS_LOCKED**
[`char`](../../../data-types.md) | Locked  ||
|| **NAME***
[`string`](../../../data-types.md)| Name of the category  ||
|| **SORT** 
[`integer`](../../../data-types.md) | Sorting   ||
|#