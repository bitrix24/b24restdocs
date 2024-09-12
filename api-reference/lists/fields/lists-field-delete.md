# Delete Field from List lists.field.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

The method `lists.field.delete` allows you to remove a field from a list. If the field is successfully deleted, it returns a response of `true`; otherwise, an *Exception* will be generated.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **IBLOCK_TYPE_ID**^*^
[`unknown`](../../data-types.md) | `id` of the information block type (required):
- **lists** - type of list information block
- **bitrix_processes** - type of processes information block
- **lists_socnet** - type of group lists information block ||
|| **IBLOCK_CODE/IBLOCK_ID**^*^
[`unknown`](../../data-types.md) | code or `id` of the information block (required) ||
|| **SOCNET_GROUP_ID**^*^
[`unknown`](../../data-types.md) | `id` of the group (required if the list is created for a group) ||
|| **FIELD_ID**^*^
[`unknown`](../../data-types.md) | `ID` of the field (required. If the field is a property of the information block, the format is: "PROPERTY_propertyId") ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Example

```javascript
var params = {
    'IBLOCK_TYPE_ID': 'lists_socnet',
    'IBLOCK_CODE': 'rest_1',
    'FIELD_ID': 'PROPERTY_61'
};
BX24.callMethod(
    'lists.field.delete',
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