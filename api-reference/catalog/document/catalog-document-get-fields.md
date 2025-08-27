# Get Warehouse Accounting Document Fields catalog.document.getFields

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameter specifications are missing
- no response in case of error
- no response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

## Description

```http
catalog.document.getFields()
```

This method returns a list of fields for warehouse accounting documents.

## Parameters

No parameters.

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.document.getFields',
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
                'catalog.document.getFields',
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
        echo 'Error getting document fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.document.getFields',
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

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** | **Description** | **Note** ||
|| **commentary** 
[`char`](../../data-types.md) | Comment. |  ||
|| **createdBy** 
[`integer`](../../data-types.md) | Created by. | Immutable field. ||
|| **currency^*^** 
[`char`](../../data-types.md) | Currency. | Immutable field. ||
|| **dateCreate** 
[`datetime`](../../data-types.md) | Creation date. | Immutable field. ||
|| **dateDocument** 
[`datetime`](../../data-types.md) | Document date. |  ||
|| **dateModify** 
[`datetime`](../../data-types.md) | Modification date. |  ||
|| **dateStatus** 
[`datetime`](../../data-types.md) | Status set date. | Read-only. ||
|| **docNumber** 
[`string`](../../data-types.md) | Document number. |  ||
|| **docType^*^**
[`char`](../../data-types.md) | Document type:
- `A` – Stock receipt; 
- `S` – Stock adjustment; 
- `M` – Transfers between warehouses; 
- `R` – Product return; 
- `D` – Write-offs. | Immutable field. ||
|| **id** 
[`integer`](../../data-types.md) | Document identifier. | Read-only. ||
|| **modifiedBy** 
[`integer`](../../data-types.md) | Modified by. |  ||
|| **responsibleId^*^** 
[`integer`](../../data-types.md) | Responsible person. |  ||
|| **status** 
[`char`](../../data-types.md) | Status. | Read-only. ||
|| **statusBy** 
[`integer`](../../data-types.md) | Status set by. |  ||
|| **title** 
[`string`](../../data-types.md) | Document title. |  ||
|| **total** 
[`double`](../../data-types.md) | Total amount of products. |  ||
|#

{% include [Footnote on parameters](../../../_includes/required.md) %}