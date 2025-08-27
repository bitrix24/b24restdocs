# Get the list of product fields for the inventory management document catalog.document.element.fields

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

{% note warning "Method development has been halted" %}

The method `catalog.document.element.fields` continues to function, but there is a more current equivalent [catalog.document.element.getFields](../document-element/catalog-document-element-get-fields.md).

{% endnote %}

The method `catalog.document.element.fields` returns a list of product fields for the inventory management document.

## Method Parameters

No parameters.

## Code Examples

{% include [Note about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.element.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.element.fields
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.document.element.fields',
    		{}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
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
                'catalog.document.element.fields',
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
        echo 'Error calling catalog document element fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.document.element.fields',
        {},
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
        'catalog.document.element.fields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue Learning 

- [{#T}](./catalog-document-confirm.md)
- [{#T}](./catalog-document-unconfirm.md)
- [{#T}](./catalog-document-element-fields.md)