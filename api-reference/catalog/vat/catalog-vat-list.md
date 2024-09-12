# Get a List of VAT Rates by Filter catalog.vat.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Required parameters are not specified
- No response in case of an error
- No examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
catalog.vat.list(select, filter, order, start)
```

The method retrieves a list of VAT rates based on the filter.

If the operation is successful, a list of VAT rates is returned in the response body.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **select** 
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-vat-get-fields.md).||
|| **filter** 
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-vat-get-fields.md). ||
|| **order**
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-vat-get-fields.md). ||
|| **start** 
[`string`](../../data-types.md)| Page number for output. Works for HTTPS requests. ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- js
  
    ```
    BX24.callMethod(
        'catalog.vat.list',
        {
            select: {
                id        
            },
            filter: {
                modifiedBy: 1
            },
            order: {
                id: "ASC"
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error().ex);
            else
                console.log(result.data());
            result.next();
        }
    );
    ```

- For HTTPS:

    ```
    https://your_account/rest/catalog.vat.list?auth=_authorization_key_&start=50
    ```

{% endlist %}

{% include [Examples Note](../../../_includes/examples.md) %}