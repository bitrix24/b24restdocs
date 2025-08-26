# Update the entity.section.update method

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- examples are missing
- success response is absent

{% endnote %}

{% endif %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `entity.section.update` method updates a storage section. The user must have at least write access permission (**W**) in the storage.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY^*^**
[`string`](../../data-types.md) | Required. String identifier of the storage. ||
|| **ID^*^**
[`integer`](../../data-types.md) | Required. Identifier of the section being updated. ||
|| **NAME**
[`string`](../../data-types.md) | Name of the section. ||
|| **DESCRIPTION**
[`unknown`](../../data-types.md) | Description of the section. ||
|| **ACTIVE**
[`unknown`](../../data-types.md) | Flag indicating the section's activity (Y\|N). ||
|| **SORT**
[`unknown`](../../data-types.md) | Sorting parameter of the section. ||
|| **PICTURE**
[`unknown`](../../data-types.md) | Picture of the section. ||
|| **DETAIL_PICTURE**
[`unknown`](../../data-types.md) | Detailed picture of the section. ||
|| **SECTION**
[`unknown`](../../data-types.md) | Identifier of the parent section. ||
|#

{% include [Parameter notes](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.section.update',
    		{
    			ENTITY: 'menu_new',
    			ID: 220,
    			NAME: 'Not a very test section'
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
                'entity.section.update',
                [
                    'ENTITY' => 'menu_new',
                    'ID'     => 220,
                    'NAME'   => 'Not a very test section',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating entity section: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.section.update',
        {
            ENTITY: 'menu_new',
            ID: 220,
            NAME: 'Not a very test section'
        }
    );
    ```

- HTTP

    ```http
    https://my.bitrix24.com/rest/entity.section.update.json?auth=9affe382af74d9c5caa588e28096e872&ENTITY=menu_new&ID=220&NAME=Not%20a%20very%20test%20section
    ```

{% endlist %}

{% include [Example notes](../../../_includes/examples.md) %}

## Success response

> 200 OK
```json
{"result":true}
```