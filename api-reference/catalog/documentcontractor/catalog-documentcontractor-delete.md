# Remove the binding of the CRM entity (Contact/Company) to the document catalog.documentcontractor.delete
{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- the required parameters are not specified
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.documentcontractor.delete(id)
```

Method for removing the vendor binding to the document. If the operation is successful, `true` is returned in the response body.

Cases when errors may occur:

- if there is no access to inventory management, no access to view stock receipt documents, or no access to edit the stock receipt document (adding/removing a binding is considered editing the document);
- if the binding with the specified id is not found.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the CRM entity binding (Contact/Company) to the document. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

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

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}