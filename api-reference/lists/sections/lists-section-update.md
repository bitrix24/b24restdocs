# Update the lists.section.update section

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- adjustments needed for writing standards
- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `lists.section.update` method updates a list section. If the item is successfully updated, the response is `true`; otherwise, it returns *Exception*.

#|
|| **Parameter** | **Description** ||
|| **IBLOCK_TYPE_ID**^*^
[`unknown`](../../data-types.md) | Identifier of the information block type (required). Possible values: 
- lists - list information block type 
- bitrix_processes - processes information block type 
- lists_socnet - group lists information block type | ||
|| **IBLOCK_CODE/IBLOCK_ID**^*^
[`unknown`](../../data-types.md) | Code or identifier of the information block (required). | ||
|| **IBLOCK_SECTION_ID**
[`unknown`](../../data-types.md) | Identifier of the parent section; if not specified, the section will be root | ||
|| **FIELDS** | Array of fields and values. Required field: NAME | ||
|| **SOCNET_GROUP_ID**^*^
[`unknown`](../../data-types.md) | `id` of the group (required if the list is created for a group); | ||
|#

{% include [Parameter notes](../../../_includes/required.md) %}

## Example

```js
/* lists.section.update */
var params = {
    'IBLOCK_TYPE_ID': 'lists',
    'IBLOCK_CODE': 'rest_1',
    'SECTION_CODE': 'Section_code_1',
    'FIELDS': {
        'NAME': 'Section_1 (Updated)'
    }
};
BX24.callMethod(
    'lists.section.update',
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

{% include [Example notes](../../../_includes/examples.md) %}