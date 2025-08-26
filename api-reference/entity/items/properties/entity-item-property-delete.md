# Delete additional property of storage items entity.item.property.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`entity`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method deletes additional properties of storage items. The user must have management rights (**X**) for the storage.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY^*^**
[`string`](../../../data-types.md) | Required. String identifier of the storage. ||
|| **PROPERTY^*^**
[`string`](../../../data-types.md) | Required. String identifier of the property. ||
|#

{% include [Note on parameters](../../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.item.property.delete',
    		{
    			ENTITY: 'menu_new',
    			PROPERTY: 'new_prop'
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
                'entity.item.property.delete',
                [
                    'ENTITY'   => 'menu_new',
                    'PROPERTY' => 'new_prop',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting entity item property: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.item.property.delete',
        {
            ENTITY: 'menu_new',
            PROPERTY: 'new_prop'
        }
    );
    ```

- HTTP

    ```http
    https://my.bitrix24.com/rest/entity.item.property.delete.json?ENTITY=menu_new&PROPERTY=new_prop&auth=d92dd12b9b9b904254776104eed2bb76
    ```

{% endlist %}

{% include [Note on examples](../../../../_includes/examples.md) %}

## Response on success

> 200 OK
```json
{"result":true}
```