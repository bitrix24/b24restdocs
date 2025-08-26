# Update Additional Property of Storage Items entity.item.property.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`entity`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `entity.item.property.update` updates the additional property of storage items. The user must have management rights (**X**) for the storage.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY^*^**
[`string`](../../../data-types.md) | Required. String identifier of the storage. ||
|| **PROPERTY^*^**
[`string`](../../../data-types.md) | Required. String identifier of the property. ||
|| **PROPERTY_NEW**
[`string`](../../../data-types.md) | New string identifier of the property. ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the property. ||
|| **TYPE**
[`unknown`](../../../data-types.md) | Type of the property (**S** - string, **N** - number, **F** - file). ||
|#

{% include [Notes on parameters](../../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.item.property.update',
    		{
    			ENTITY: 'menu_new',
    			PROPERTY: 'new_prop',
    			NAME: 'Not a new property anymore'
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
                'entity.item.property.update',
                [
                    'ENTITY'   => 'menu_new',
                    'PROPERTY' => 'new_prop',
                    'NAME'     => 'Not a new property anymore'
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating entity item property: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.item.property.update',
        {
            ENTITY: 'menu_new',
            PROPERTY: 'new_prop',
            NAME: 'Not a new property anymore'
        }
    );
    ```

- HTTP

    ```http
    https://my.bitrix24.com/rest/entity.item.property.update.json?ENTITY=menu_new&NAME=Not%20a%20new%20property%20anymore&PROPERTY=new_prop&auth=ad5a6f34f14f644136830eb8a936f07f
    ```

{% endlist %}

{% include [Notes on examples](../../../../_includes/examples.md) %}

## Response on Success

> 200 OK
```json
{"result":true}
```