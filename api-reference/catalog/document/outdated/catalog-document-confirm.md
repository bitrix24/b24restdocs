# Conduct the inventory management document catalog.document.confirm

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

{% note warning "Method development has been halted" %}

The method `catalog.document.confirm` continues to function, but there is a more relevant alternative [catalog.document.conduct](../catalog-document-conduct.md).

{% endnote %}

The method `catalog.document.confirm` conducts the inventory management document.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md)| Document identifier ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":42}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.confirm
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":42,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.confirm
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.document.confirm',
    		{
    			'id': 42,
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    		console.error(result.error());
    	else
    		console.log(result);
    }
    catch(error)
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.document.confirm',
                [
                    'id' => 42,
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
        echo 'Error confirming document: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
    'catalog.document.confirm',
    {
        'id': 42,
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.log(result.data());
    }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.document.confirm',
        [
            'id' => 42
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue Learning 

- [{#T}](./catalog-document-unconfirm.md)
- [{#T}](./catalog-document-fields.md)
- [{#T}](./catalog-document-element-fields.md)