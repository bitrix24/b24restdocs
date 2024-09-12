# Get Infoblock Data lists.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `lists.get` method returns infoblock data. On success, the infoblock data will be returned; otherwise, an empty array will be returned. This method allows you to retrieve a list of data from all infoblocks of the specified type at once. When working with social network groups, it is essential to specify the group's `ID`, or an access error will occur.

## Parameters
#|
|| **Parameter** | **Description** ||
|| **IBLOCK_TYPE_ID**^*^
[`unknown`](../../data-types.md) | `id` of the infoblock type (required):
- **lists** - list infoblock type
- **bitrix_processes** - processes infoblock type
- **lists_socnet** - lists infoblock type for groups ||
|| **IBLOCK_CODE/IBLOCK_ID**
[`unknown`](../../data-types.md) | code or `id` of the infoblock (if not specified, data for all lists of the specified infoblock type will be returned) ||
|| **SOCNET_GROUP_ID**
[`unknown`](../../data-types.md) | `id` of the group, required if the list is in groups. ||
|| **IBLOCK_ORDER**
[`unknown`](../../data-types.md) | Sorting. An array of infoblock section fields. Sorting direction: **asc** (ascending) or **desc** (descending). Example: 
`'IBLOCK_ORDER': { "ID": "DESC" }` ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Example

```js
var params = {
    'IBLOCK_TYPE_ID': 'lists',
    'IBLOCK_CODE': 'rest_1'
};
BX24.callMethod(
    'lists.get',
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

{% include [Example Notes](../../../_includes/examples.md) %}