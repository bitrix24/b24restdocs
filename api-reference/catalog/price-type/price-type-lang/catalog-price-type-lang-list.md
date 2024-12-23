# Get a List of Translations for Price Type Names by Filter catalog.priceTypeLang.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameter specifications are missing
- no response in case of an error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
catalog.priceTypeLang.list(select, filter, order, start)
```

This method retrieves a list of translations for price type names based on the filter.


## Parameters

#|
|| **Parameter** | **Description** ||
|| **select** 
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](./catalog-price-type-lang-get-fields.md).||
|| **filter** 
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](./catalog-price-type-lang-get-fields.md). ||
|| **order**
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](./catalog-price-type-lang-get-fields.md). ||
|| **start** 
[`string`](../../data-types.md)| Page number for output. Works for HTTPS requests. ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'catalog.priceTypeLang.list',
        {
            filter: {
                catalogGroupId: 1
            },
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

{% endlist %}

Example HTTPS request

```
https://your_account/rest/catalog.priceTypeLang.list?auth=_authorization_key_&start=50
```

{% include [Example Notes](../../../../_includes/examples.md) %}