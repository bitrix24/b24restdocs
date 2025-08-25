# Get Custom Field Fields task.item.userfield.getfields

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing examples (should have three examples - curl, js, php)
- missing success response
- missing error response

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.item.userfield.getfields` returns all available fields.

## Parameters

No parameters.

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'task.item.userfield.getfields',
    		{}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    	console.log(result);
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
                'task.item.userfield.getfields',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting user fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.item.userfield.getfields',
        {},
        function(result)
        {
            console.info(result.data());
            console.log(result);
        }
    );
    ```

{% endlist %}

{% include [Examples note](../../../_includes/examples.md) %}

## List of Fields

#|
|| **Code** / **Type** | **Field** | **Note** ||
|| **ID**
[`int`](../../data-types.md) | Identifier | Read-only ||
|| **ENTITY_ID**
[`string`](../../data-types.md) | Object ||
|| **FIELD_NAME**
[`string`](../../data-types.md) | Code | Immutable ||
|| **USER_TYPE_ID**
[`string`](../../data-types.md) | Data type | Immutable ||
|| **XML_ID**
[`string`](../../data-types.md) | External identifier (XML ID) ||
|| **SORT**
[`int`](../../data-types.md) | Sorting ||
|| **MULTIPLE**
[`char`](../../data-types.md) | Multiple ||
|| **MANDATORY**
[`char`](../../data-types.md) | Mandatory ||
|| **SHOW_FILTER**
[`char`](../../data-types.md) | Show in list filter ||
|| **SHOW_IN_LIST**
[`char`](../../data-types.md) | Show in list ||
|| **EDIT_IN_LIST**
[`char`](../../data-types.md) | Allow user editing ||
|| **IS_SEARCHABLE**
[`char`](../../data-types.md) | Field values participate in search ||
|| **EDIT_FORM_LABEL**
[`string`](../../data-types.md) | Label in edit form ||
|| **LIST_COLUMN_LABEL**
[`string`](../../data-types.md) | Header in list ||
|| **LIST_FILTER_LABEL**
[`string`](../../data-types.md) | Filter label in list ||
|| **ERROR_MESSAGE**
[`string`](../../data-types.md) | Error message ||
|| **HELP_MESSAGE**
[`string`](../../data-types.md) | Help ||
|| **LIST**
[`uf_enum_element`](../../data-types.md) | List elements | Multiple ||
|| **SETTINGS**
[`object`](../../data-types.md) | Additional settings (depend on type) ||
|#