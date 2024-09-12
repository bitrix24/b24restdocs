# Update List Field `lists.field.update`

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

The method `lists.field.update` allows you to update a list field. Upon successful field update, the response is `true`, otherwise *Exception*.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **IBLOCK_TYPE_ID**^*^
[`unknown`](../../data-types.md) | `id` of the information block type (required):
- **lists** - list information block type
- **bitrix_processes** - processes information block type
- **lists_socnet** - group lists information block type ||
|| **IBLOCK_CODE/IBLOCK_ID**^*^
[`unknown`](../../data-types.md) | code or `id` of the information block (required) ||
|| **SOCNET_GROUP_ID**^*^
[`unknown`](../../data-types.md) | `id` of the group (required if the list is created for a group) ||
|| **FIELD_ID**^*^
[`unknown`](../../data-types.md) | `ID` of the field (required. If the field is a property of the information block, the format is: "PROPERTY_propertyId") ||
|| **FIELDS**
[`unknown`](../../data-types.md) | keys are the same as when creating a field from the Bitrix24 interface, including:
- **NAME**^*^ - name (required)
- **IS_REQUIRED** - required flag
- **MULTIPLE** - multiple flag
- **TYPE**^*^ - type (required)
- **SORT** - sorting
- **DEFAULT_VALUE** - default value
- **LIST** - for adding values to a "List" type field
- **LIST_TEXT_VALUES** - for adding values to a "List" type field using a string
- **LIST_DEF** - default value for a "List" type field
- **CODE**^*^ - code (required if the field is a property of the information block)
- **SETTINGS** - field display settings
- **USER_TYPE_SETTINGS** - user settings
- **ROW_COUNT/COL_COUNT** - settings for textarea fields
- **LINK_IBLOCK_ID** - `id` of the linked section ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Example

```js
var params = {
    'IBLOCK_TYPE_ID': 'lists_socnet',
    'IBLOCK_CODE': 'rest_1',
    'FIELD_ID': 'PROPERTY_61',
    'FIELDS': {
        'NAME': 'List field (Update)',
        'IS_REQUIRED': 'N',
        'MULTIPLE': 'N',
        'TYPE': 'L',
        'SORT': '20',
        'CODE': 'fieldList',
        'LIST': {
            '58': {
                'SORT': '10',
                'VALUE': 'one'
            },
            '59': {
                'SORT': '20',
                'VALUE': 'two'
            },
            '60': {
                'SORT': '30',
                'VALUE': 'three'
            }
        },
        'LIST_DEF': {
            '0': '59'
        },
        'SETTINGS': {
            'SHOW_ADD_FORM': 'Y',
            'SHOW_EDIT_FORM': 'Y',
            'ADD_READ_ONLY_FIELD': 'N',
            'EDIT_READ_ONLY_FIELD': 'Y',
            'SHOW_FIELD_PREVIEW': 'N'
        }
    }
};
BX24.callMethod(
    'lists.field.update',
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

{% include [Example Notes](../../../_includes/examples.md) %}