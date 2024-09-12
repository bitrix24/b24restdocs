# Remove the binding of the CRM entity (Contact/Company) to the document catalog.documentcontractor.delete
{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- the required parameters are not specified
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can perform the method: any user

## Description

```http
catalog.documentcontractor.delete(id)
```

This method is used to remove the vendor binding to the document. If the operation is successful, it returns `true` in the response body.

Cases when errors may occur:

- if there is no access to inventory accounting, no access to view incoming documents, or no access to edit the incoming document (adding/removing a binding is considered editing the document);
- if the binding with the specified id is not found.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the CRM entity (Contact/Company) binding to the document. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```js
BX.callMethod(
    'catalog.documentcontractor.delete',
    { id: 20 },
    function(result) {
        if (result.error())
            console.error(result.error().ex);
        else
            console.log(result.data());
    }
);
```

{% include [Note on examples](../../../_includes/examples.md) %}