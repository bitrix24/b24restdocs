# Get a List of Images for a Specific Product catalog.productImage.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- the required parameters are not specified
- there is no response in case of an error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
catalog.productImage.list(productId, select, start)
```

This method retrieves a list of images for a specific product or trade offer.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **productId^*^** 
[`string`](../../data-types.md)| The identifier of the product or trade offer.||
|| **select** 
[`object`](../../data-types.md)| A list of [`fields`](catalog-product-image-get-fields.md) to display in the response. ||
|| **start** 
[`string`](../../data-types.md)| The page number for output. Works for HTTPS requests. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

For JS

```javascript
BX24.callMethod(
    'catalog.productImage.list',
    {
        productId: 1
    },
    function(result) {
        if(result.error())
            console.error(result.error().ex);
        else
            console.log(result.data());
        result.next();
    }
);
```

Example HTTPS request

```
https://your_account/rest/catalog.productImage.list?auth=_authorization_key_&start=50
```

{% include [Note on examples](../../../_includes/examples.md) %}