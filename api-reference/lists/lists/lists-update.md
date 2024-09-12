# Update Current Universal List lists.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `lists.update` method updates an existing list. If the list is successfully updated, the response is `true`; otherwise, it returns *Exception*.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **IBLOCK_TYPE_ID**^*^
[`unknown`](../../data-types.md) | `id` of the information block type (required):
- **lists** - list information block type
- **bitrix_processes** - processes information block type
- **lists_socnet** - group lists information block type ||
|| **IBLOCK_CODE/IBLOCK_ID**^*^
[`unknown`](../../data-types.md) | code or `id` of the information block (required); ||
|| **SOCNET_GROUP_ID**^*^
[`unknown`](../../data-types.md) | `id` of the group (required if the list is created for a group); ||
|| **FIELDS**
[`unknown`](../../data-types.md) | information block fields:
- **NAME**^*^ - name of the information block (required);
- **DESCRIPTION** - description;
- **SORT** - sorting;
- **PICTURE** - image;
- **BIZPROC** - enable business process support. ||
|| **MESSAGES**
[`unknown`](../../data-types.md) | labels for list items and sections; ||
|| **RIGHTS**
[`unknown`](../../data-types.md) | manage access permissions. ||
|#

{% include [Parameter Footnote](../../../_includes/required.md) %}

## Example

```javascript
var params = {
    'IBLOCK_TYPE_ID': 'lists_socnet',
    'IBLOCK_CODE': 'rest_1',
    'FIELDS': {
        'NAME': 'List 1 (Update)',
        'DESCRIPTION': 'Test list (Update)',
        'SORT': '20',
        'PICTURE': document.getElementById('iblock-image-update')
    },
    'RIGHTS': {
        'G1': 'X'
    }
};
BX24.callMethod(
    'lists.update',
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

{% include [Example Footnote](../../../_includes/examples.md) %}