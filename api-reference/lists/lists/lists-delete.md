# Delete Universal List lists.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `lists.delete` method removes a list. Upon successful deletion, the response is *true*, otherwise it is *false*.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **IBLOCK_TYPE_ID**^*^
[`unknown`](../../data-types.md) | `id` of the information block type (required):
- **lists** - information block type for lists
- **bitrix_processes** - information block type for processes
- **lists_socnet** - information block type for group lists ||
|| **IBLOCK_CODE/IBLOCK_ID**^*^
[`unknown`](../../data-types.md) | code or `id` of the information block (required); ||
|| **SOCNET_GROUP_ID**
[`unknown`](../../data-types.md) | `id` of the group (required if the list is created for a group); ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Example

```javascript
var params = {
    'IBLOCK_TYPE_ID': 'lists_socnet',
    'IBLOCK_CODE': 'rest_1'
};
BX24.callMethod(
    'lists.delete',
    params,
    function(result)
    {
        if(result.error())
            alert("Error: " + result.error());
        else
            alert("Success: " + result.data());
    }
);
```

{% include [Example Note](../../../_includes/examples.md) %}