# Add Additional Property to Storage Elements `entity.item.property.add`

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`entity`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `entity.item.property.add` adds an additional property to storage elements. The user must have management rights (**X**) for the storage.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY^*^**
[`string`](../../../data-types.md) | Required. String identifier of the storage. ||
|| **PROPERTY^*^**
[`string`](../../../data-types.md) | Required. String identifier of the property. ||
|| **NAME^*^**
[`string`](../../../data-types.md) | Required. Name of the property. ||
|| **TYPE^*^**
[`unknown`](../../../data-types.md) | Required. Type of the property (**S** - string, **N** - number, **F** - file). ||
|#

{% include [Note on parameters](../../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.item.property.add',
    		{
    			ENTITY: 'menu_new',
    			PROPERTY: 'new_prop',
    			NAME: 'New Property',
    			TYPE: 'S'
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log('Created element with ID:', result);
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
                'entity.item.property.add',
                [
                    'ENTITY'   => 'menu_new',
                    'PROPERTY' => 'new_prop',
                    'NAME'     => 'New Property',
                    'TYPE'     => 'S',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding entity item property: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.item.property.add',
        {
            ENTITY: 'menu_new',
            PROPERTY: 'new_prop',
            NAME: 'New Property',
            TYPE: 'S'
        }
    );
    ```

- HTTP

    ```http
    https://my.bitrix24.com/rest/entity.item.property.add.json?ENTITY=menu_new&NAME=New%20Property&PROPERTY=new_prop&TYPE=S&auth=e690b44d2b3827d2eb9d4dbe59406dbb
    ```

{% endlist %}

{% include [Note on examples](../../../../_includes/examples.md) %}

## Response on Success

> 200 OK
```json
{"result":true}
```