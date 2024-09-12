# Get a List of Measurement Unit Ratios by Filter catalog.ratio.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of an error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
catalog.ratio.list(select, filter, order, start)
```

This method retrieves a list of measurement unit ratios based on the filter.

If the operation is successful, a list of measurement unit ratios is returned in the response body.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **select** 
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-ratio-get-fields.md). ||
|| **filter** 
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-ratio-get-fields.md). ||
|| **order** 
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-ratio-get-fields.md). ||
|| **start** 
[`string`](../../data-types.md)| Page number for output. Works for HTTPS requests. ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- js
  
    ```
    BX24.callMethod(
    'catalog.ratio.list',
    {
        select:{
            id
        },
        filter:{
            isDefault: "Y"
        },
        order:{
            id: "asc"
        },
        start: 1
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
    https://your_account/rest/catalog.ratio.list?auth=_authorization_key_&start=50
    ```

{% endlist %}

{% include [Example Notes](../../../_includes/examples.md) %}