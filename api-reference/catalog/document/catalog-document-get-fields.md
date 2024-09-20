# Get Inventory Management Document Fields catalog.document.getFields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
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

This method returns a list of fields for inventory management documents.

## Parameters

No parameters.

## Examples

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

{% include [Example Notes](../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** | **Description** | **Note** ||
|| **commentary** 
[`char`](../../data-types.md) | Commentary. |  ||
|| **contractorId** 
[`integer`](../../data-types.md) | Vendor. |  ||
|| **createdBy** 
[`integer`](../../data-types.md) | Created by. |  Read-only field. ||
|| **currency^*^** 
[`char`](../../data-types.md) | Currency. | Read-only field. ||
|| **dateCreate** 
[`datetime`](../../data-types.md) | Creation date. | Read-only field. ||
|| **dateDocument** 
[`datetime`](../../data-types.md) | Document execution date. |  ||
|| **dateModify** 
[`datetime`](../../data-types.md) | Modification date. |  ||
|| **dateStatus** 
[`datetime`](../../data-types.md) | Status set date. | Read-only. ||
|| **docNumber** 
[`string`](../../data-types.md) | Document number. |  ||
|| **docType^*^**
[`char`](../../data-types.md) | Document type:
- `A` – Inventory receipt; 
- `S` – Stock adjustment; 
- `M` – Transfers between inventories; 
- `R` – Product return; 
- `D` – Write-offs. |  Read-only field. ||
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
[`double`](../../data-types.md) | Total amount of goods. |  ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}