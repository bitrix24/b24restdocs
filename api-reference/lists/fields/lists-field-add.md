# Create a List Field lists.field.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing
- response on success is absent
- response on error is absent

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `lists.field.add` creates a list field. On successful creation of the field, the response is `true`, otherwise `Exception`.

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
[`unknown`](../../data-types.md) | `id` of the group (required if the list is created for a group); ||
|| **FIELDS**
[`unknown`](../../data-types.md) | keys are the same as when creating a field from the Bitrix24 interface:
- **NAME**^*^ - name (required)
- **IS_REQUIRED** - required flag
- **MULTIPLE** - multiple flag
- **TYPE**^*^ - type (required), including:
    - **S** - String
    - **N** - Number
    - **L** - List
    - **F** - File
    - **G** - Section binding
    - **E** - Element binding
    - **S:Date** - Date
    - **S:DateTime** - Date/Time
    - **S:HTML** - HTML/text
    - **E:EList** - Element binding as a list. When creating a field with this type, it is necessary to specify `LINK_IBLOCK_ID` - `id` of the information block whose elements will be displayed in the selector of this field.
    - **N:Sequence** - Counter
    - **S:Money** - Money
    - **S:DiskFile** - File (Disk)
    - **S:map_yandex** - Yandex.Map binding
    - **S:employee** - Employee binding
    - **S:ECrm** - CRM element binding
- **SORT** - sorting
- **DEFAULT_VALUE** - default value
- **LIST** - can be used to add values for the "List" type field.
- **LIST_TEXT_VALUES** - can be used to add values for the "List" type field using a string. (Each unique line will become a separate list value)
- **LIST_DEF** - default value for the "List" type field (Format: array with the value where the value is the `id` of the list item)
- **CODE**^*^ - code (required if the field is a property of the information block)
- **SETTINGS** - all keys must be present, otherwise default values will overwrite, including:
    - **SHOW_ADD_FORM** - show in the add form
    - **SHOW_EDIT_FORM** - show in the edit form
    - **ADD_READ_ONLY_FIELD** - read-only (add form)
    - **EDIT_READ_ONLY_FIELD** - read-only (edit form)
    - **SHOW_FIELD_PREVIEW** - show the field when generating a link to the list item
- **USER_TYPE_SETTINGS** - key for passing custom settings
- **ROW_COUNT/COL_COUNT** - settings for textarea fields
- **LINK_IBLOCK_ID** - `id` of the linked list (section of the information block) ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Example

```javascript
var params = {
    'IBLOCK_TYPE_ID': 'lists_socnet',
    'IBLOCK_CODE': 'rest_1',
    'SOCNET_GROUP_ID': '1',
    'FIELDS': {
        'NAME': 'List field',
        'IS_REQUIRED': 'Y',
        'MULTIPLE': 'N',
        'TYPE': 'L',
        'SORT': '20',
        'CODE': 'fieldList',
        'LIST_TEXT_VALUES': 'one\ntwo\nthree',
        'SETTINGS': {
            'SHOW_ADD_FORM': 'Y',
            'SHOW_EDIT_FORM': 'Y',
            'ADD_READ_ONLY_FIELD': 'N',
            'EDIT_READ_ONLY_FIELD': 'N',
            'SHOW_FIELD_PREVIEW': 'N'
        }
    }
};
BX24.callMethod(
    'lists.field.add',
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