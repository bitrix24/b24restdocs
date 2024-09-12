# Get a list of CRM entity (Contact/Company) bindings to documents catalog.documentcontractor.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- the requiredness of parameters is not specified
- no response in case of an error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
catalog.documentcontractor.list(select, filter, order, start)
```

The method retrieves a list of CRM entity (Contact/Company) bindings to documents based on the filter.

Returns a list of bindings with the structure described in the **select** parameter (if not specified, all fields are returned, as in the [getFields](catalog-documentcontractor-get-fields.md) method).

## Parameters

#|
|| **Parameter** | **Description** ||
|| **select**
[`object`](../../data-types.md) | Fields corresponding to the available list of fields [fields](catalog-documentcontractor-get-fields.md). ||
|| **filter**
[`object`](../../data-types.md) | Fields corresponding to the available list of fields [fields](catalog-documentcontractor-get-fields.md). ||
|| **order**
[`object`](../../data-types.md) | Fields corresponding to the available list of fields [fields](catalog-documentcontractor-get-fields.md). ||
|| **start**
[`string`](../../data-types.md) | Page number for output. Works for HTTPS requests. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- js
  
    ```
    BX.callMethod(
        'catalog.documentcontractor.list',
        {
            select: ['id', 'documentId'],
            order: {'id': 'desc'},
            filter:
            {
                'documentId': 6,
                'entityTypeId': 3
            }
        },

        function(result)
        {
            console.log(result.data())
            result.next();
        }
    );
    ```
- For HTTPS:

    ```
    https://your_account/rest/catalog.documentcontractor.list?auth=_authorization_key_&start=50
    ```

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}