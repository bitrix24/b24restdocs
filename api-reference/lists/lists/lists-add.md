# Create a universal list lists.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed to meet writing standards
- parameter types are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `lists.add` creates a list. If the list is created successfully, the response is `true`, otherwise *Exception*.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **IBLOCK_TYPE_ID**^*^
[`unknown`](../../data-types.md) | `id` of the information block (required):
- **lists** - information block type for lists
- **bitrix_processes** - information block type for processes
- **lists_socnet** - information block type for group lists ||
|| **IBLOCK_CODE**^*^
[`unknown`](../../data-types.md) | code of the information block (required); ||
|| **SOCNET_GROUP_ID**^*^
[`unknown`](../../data-types.md) | `id` of the group (required if the list is created for a group); ||
|| **FIELDS**
[`unknown`](../../data-types.md) | fields of the information block:
- **NAME**^*^ - name of the information block (required);
- **DESCRIPTION** - description;
- **SORT** - sorting;
- **PICTURE** - image;
- **BIZPROC** - enabling support for business processes. ||
|| **MESSAGES**
[`unknown`](../../data-types.md) | labels for list items and sections; ||
|| **RIGHTS**
[`unknown`](../../data-types.md) | access permission management (if not filled, the authorized user working with REST is granted full access to the created information block). ||
|#

{% include [Parameter notes](../../../_includes/required.md) %}

## Example

```js
var params = {
    'IBLOCK_TYPE_ID': 'lists_socnet',
    'IBLOCK_CODE': 'rest_1',
    'SOCNET_GROUP_ID': '4',
    'FIELDS': {
        'NAME': 'List 1',
        'DESCRIPTION': 'Test list',
        'SORT': '10',
        'PICTURE': document.getElementById('iblock-image-add'),
        'BIZPROC': 'Y'
    },
    'MESSAGES': {
        'ELEMENT_NAME': 'Element',
        'ELEMENTS_NAME': 'Elements',
        'ELEMENT_ADD': 'Add element',
        'ELEMENT_EDIT': 'Edit element',
        'ELEMENT_DELETE': 'Delete element',
        'SECTION_NAME': 'Section',
        'SECTIONS_NAME': 'Sections',
        'SECTION_ADD': 'Add section',
        'SECTION_EDIT': 'Edit section',
        'SECTION_DELETE': 'Delete section'
    },
    'RIGHTS': {
        'SG4_A': 'W',
        'SG4_E': 'W',
        'SG4_K': 'W',
        'AU': 'D',
        'G2': 'D'
    }
};
BX24.callMethod(
    'lists.add',
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

{% include [Example notes](../../../_includes/examples.md) %}