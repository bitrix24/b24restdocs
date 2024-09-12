# Change Values of Custom Fields for Inventory Documents catalog.userfield.document.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of error
- no examples in other languages
- add a link to [`userfieldconfig.list`](.)
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
catalog.userfield.document.update(documentId, fields)
```

This method updates the values of custom fields for inventory documents.

## Parameters

#|
|| **Parameter** | **Description**  ||
|| **documentId** 
[`integer`](../../data-types.md) | Identifier of the inventory document. | ||
|| **fields** 
[`object`](../../data-types.md)| Fields to be updated and their new values. `documentType` must be specified – [type of inventory documents](../enum/catalog-enum-get-store-document-types.md). | ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

### Example

In the API, field names are represented as `field[Field ID in the database]` – for example, `field287`. The field ID can be obtained using the method [`userfieldconfig.list`](.).

```js
BX24.callMethod(
    'catalog.userfield.document.update',
    {
        documentId: 64,
        fields: {
            'documentType': 'S',
            'field287': 'new value'
        }
    },
    function(result)
    {
        if(result.error())
            console.error(result.error().ex);
        else
            console.log(result.data());
    }
);
```

{% include [Note on examples](../../../_includes/examples.md) %}