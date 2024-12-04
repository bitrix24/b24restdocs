# Get the Information Block Type ID lists.get.iblock.type.id

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `lists.get.iblock.type.id` returns the `id` of the information block type. On success, a string with the `id` of the information block type will be returned; otherwise, `null`.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **IBLOCK_CODE/IBLOCK_ID**
[`unknown`](../../data-types.md) | code or `id` of the information block | ||
|#

## Example:

{% list tabs %}

- JS

    ```js
    var params = {
        'IBLOCK_ID': '41'
    };
    BX24.callMethod(
        'lists.get.iblock.type.id',
        params,
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                console.log(result.data());
        }
    );
    ```

{% endlist %}



{% include [Footnote about examples](../../../_includes/examples.md) %}