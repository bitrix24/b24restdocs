# Update deal category crm.dealcategory.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`crm.category.update`](../../universal/category/crm-category-update.md)

{% endnote %}

The method updates an existing deal category.

## Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`integer`](../../../data-types.md)| Identifier of the category ||
|| **fields**
[`array`](../../../data-types.md) | Field values for updating the deal category.

To find out the required format of the fields, execute the method [`crm.dealcategory.fields`](./crm-deal-category-fields.md) and check the format of the incoming values of these fields (except for fields marked with the attributes **isReadOnly** and **isImmutable**) ||
|#

## Code examples

{% include [Note about examples](../../../../_includes/examples.md) %}

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
    try
    {
    	const id = prompt("Enter ID");
    	const sort = prompt("Enter sort order");
    	const parsedSort = parseInt(sort);
    	
    	if(isNaN(parsedSort) || parsedSort < 0)
    	{
    		parsedSort = 0;
    	}
    	
    	const response = await $b24.callMethod(
    		"crm.dealcategory.update",
    		{
    			id: id,
    			fields: { "SORT": parsedSort }
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.info(result);
    	}
    }
    catch(error)
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    $id = (int)readline("Enter ID");
    $sort = (int)readline("Enter sort order");
    if (is_nan($sort) || $sort < 0) {
        $sort = 0;
    }
    
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.dealcategory.update',
                [
                    'id'     => $id,
                    'fields' => ['SORT' => $sort],
                ]
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
        echo 'Error updating deal category: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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