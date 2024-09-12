# Get Available Field Types for lists.field.type.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon.

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

The method `lists.field.type.get` allows you to retrieve a list of available field types for the specified list. On success, a list of available field types will be returned; otherwise, an empty array will be returned.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **IBLOCK_TYPE_ID**^*^
[`unknown`](../../data-types.md) | `id` of the information block type (required):
- **lists** - information block type for lists
- **bitrix_processes** - information block type for processes
- **lists_socnet** - information block type for group lists ||
|| **IBLOCK_CODE/IBLOCK_ID**^*^
[`unknown`](../../data-types.md) | code or `id` of the information block (required) ||
|| **SOCNET_GROUP_ID**^*^
[`unknown`](../../data-types.md) | `id` of the group (required if the list is created for a group) ||
|| **FIELD_ID**
[`unknown`](../../data-types.md) | `ID` of the field (If the field is a property of the information block, the format is: `PROPERTY_propertyId`. If specified, it returns available types for the specified field type) ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Example

```js
var params = {
    'IBLOCK_TYPE_ID': 'lists_socnet',
    'IBLOCK_CODE': 'rest_1'
};
if(!params.IBLOCK_CODE && !params.IBLOCK_ID)
    return;
BX24.callMethod(
    'lists.field.type.get',
    params,
    function(result)
    {
        if(result.error())
        {
            alert("getFieldTypes: " + result.error());
        }
        else
        {
            var types = result.data(), html = '';
            for(var typeId in types)
            {
                if(!types.hasOwnProperty(typeId)) continue;
                    html += ''+types[typeId]+'';
            }
            $('#field-type').html(html);
        }
    }
);
```

{% include [Example Note](../../../_includes/examples.md) %}