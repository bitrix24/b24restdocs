# Delete entity storage item: entity.item.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `entity.item.delete` removes a storage item. The user must have at least write access permission (**W**) in the storage.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY^*^**
[`string`](../../data-types.md) | Required. String identifier of the storage. ||
|| **ID^*^**
[`integer`](../../data-types.md) | Required. Identifier of the item. ||
|#

{% include [Parameter notes](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.item.delete',
    		{
    			ENTITY: 'menu_new',
    			ID: 842
    		}
    	);
    	
    	const result = response.getData().result;
    }
    catch( error )
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
                'entity.item.delete',
                [
                    'ENTITY' => 'menu_new',
                    'ID'     => 842
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting entity item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.item.delete',
        {
            ENTITY: 'menu_new',
            ID: 842
        }
    );
    ```

- HTTP

    ```http
    https://my.bitrix24.com/rest/entity.item.delete.json?ENTITY=menu_new&ID=842&auth=340bf57f35ee95e0debf98399632999c
    ```

{% endlist %}

{% include [Example notes](../../../_includes/examples.md) %}

## Response on success

> 200 OK
```json
{"result":true}
```