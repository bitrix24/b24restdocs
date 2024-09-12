# Get Fields with CRM Vendor Bindings to Warehouse Documents catalog.documentcontractor.getFields

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no response in case of error
- no response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```js
catalog.documentcontractor.getFields()
```

The method returns an array of fields with bindings.

## Parameters

No parameters.

## Examples

```js
BX.callMethod(
    'catalog.documentcontractor.getFields',
    {},
    function(result) {
        if (result.error())
            console.error(result.error().ex);
        else
            console.log(result.data());
    }
);
```

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** | **Description** | **Note** ||
|| **documentId^*^**
[`integer`](../../data-types.md) | Identifier of the document to which the vendor is bound. |  ||
|| **entityTypeId^*^**
[`integer`](../../data-types.md) | Identifier of the CRM entity type (Contact or Company). |  ||
|| **entityId^*^**
[`integer`](../../data-types.md) | Identifier of the CRM entity. |  ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the entity binding to the document. | Read-only. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}