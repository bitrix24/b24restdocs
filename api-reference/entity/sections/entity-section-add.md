# Add entity.section.add storage section

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `entity.section.add` method adds a storage section. The user must have at least write access permission (**W**) in the storage.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Required. String identifier of the storage. ||
|| **NAME**^*^
[`string`](../../data-types.md) | Required. Name of the section. ||
|| **DESCRIPTION**
[`unknown`](../../data-types.md) | Description of the section. ||
|| **ACTIVE**
[`unknown`](../../data-types.md) | Flag indicating if the section is active (Y\|N). ||
|| **SORT**
[`unknown`](../../data-types.md) | Sorting parameter for the section. ||
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
    		'entity.section.add',
    		{
    			ENTITY: 'menu_new',
    			'NAME': 'Test Section'
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
                'entity.section.add',
                [
                    'ENTITY' => 'menu_new',
                    'NAME'   => 'Test Section',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding entity section: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.section.add',
        {
            ENTITY: 'menu_new',
            'NAME': 'Test Section'
        }
    );
    ```

- HTTP

    ```http
    https://my.bitrix24.com/rest/entity.section.add.json?ENTITY=menu_new&NAME=Test%20Section&auth=9affe382af74d9c5caa588e28096e872
    ```

{% endlist %}

{% include [Example notes](../../../_includes/examples.md) %}

## Response on success

> 200 OK
```json
{"result":220}
```