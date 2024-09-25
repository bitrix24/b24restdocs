# Get Document Fields List (deprecated) catalog.document.fields

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of error
- no response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

{% note info "Attention!" %}

This method has been deprecated since version **22.400.0**. It is recommended to use the method [catalog.document.getFields](./catalog-document-get-fields.md).

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

## Description

```http
catalog.document.fields()
```

This method returns a list of document fields.

## Parameters

No parameters.

## Examples

{% list tabs %}

- js
  
    ```
    BX24.callMethod(
        'catalog.document.fields',
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

- php
  
    ```
    $result = CRest::call(
        'catalog.document.fields'
    );
    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

{% include [Examples Note](../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** | **Description** | **Note** ||
|| **id** 
[`integer`](../../data-types.md) | Identifier. | ||
|| **docType** 
[`char`](../../data-types.md) | Document type. |  ||
|| **siteId** 
[`char`](../../data-types.md) | Site binding. |  ||
|| **contractorId** 
[`integer`](../../data-types.md) | Vendor. |  ||
|| **responsibleId^*^** 
[`integer`](../../data-types.md) | Assignee. |  ||
|| **dateModify** 
[`datetime`](../../data-types.md) | Modification date. |  ||
|| **dateCreate** 
[`datetime`](../../data-types.md) | Creation date. |  ||
|| **createdBy** 
[`integer`](../../data-types.md) | Creator. |  ||
|| **modifiedBy** 
[`integer`](../../data-types.md) | Modified by. |  ||
|| **currency^*^** 
[`char`](../../data-types.md) | Currency. |  ||
|| **status** 
[`char`](../../data-types.md) | Status. |  ||
|| **dateStatus** 
[`datetime`](../../data-types.md) | Status set date. | ||
|| **dateDocument** 
[`datetime`](../../data-types.md) | Document date. | ||
|| **statusBy** 
[`integer`](../../data-types.md) | Status set by. |  ||
|| **total** 
[`double`](../../data-types.md) | Total amount of goods. |  ||
|| **commentary** 
[`char`](../../data-types.md) | Commentary. |  ||
|#

{% include [Parameters Note](../../../_includes/required.md) %}