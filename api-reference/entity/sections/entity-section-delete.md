# Delete the entity.section.delete storage section

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly

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

The `entity.section.delete` method removes a storage section. The user must have at least write access (**W**) to the storage.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY^*^**
[`string`](../../data-types.md) | Required. String identifier of the storage. ||
|| **ID**^*^
[`integer`](../../data-types.md) | Required. Identifier of the section to be deleted. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.section.delete',
    		{
    			ENTITY: 'menu_new',
    			ID: 220
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
                'entity.section.delete',
                [
                    'ENTITY' => 'menu_new',
                    'ID'     => 220
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting entity section: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'entity.section.delete',
        {
            ENTITY: 'menu_new',
            ID: 220
        }
    );
    ```

- HTTP

    ```http
    https://my.bitrix24.com/rest/entity.section.delete.json?ENTITY=menu_new&ID=220&auth=9affe382af74d9c5caa588e28096e872
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response on success

> 200 OK
```json
{"result":true}
```