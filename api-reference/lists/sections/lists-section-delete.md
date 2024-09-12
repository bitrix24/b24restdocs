# Delete Section of Universal List lists.section.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed to meet writing standards
- parameter types not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `lists.section.delete` removes a section from the list. If the section is successfully created, the response is `true`, otherwise *Exception*.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **IBLOCK_TYPE_ID**^*^
[`unknown`](../../data-types.md) | Identifier of the information block type (required). Possible values: 
- lists - list information block type 
- bitrix_processes - processes information block type 
- lists_socnet - group lists information block type | ||
|| **IBLOCK_CODE/IBLOCK_ID**^*^
[`unknown`](../../data-types.md) | Code or identifier of the information block (required). | ||
|| **SECTION_CODE/SECTION_ID**^*^
[`unknown`](../../data-types.md) | Code or identifier of the section (required). | ||
|| **SOCNET_GROUP_ID**^*^
[`unknown`](../../data-types.md) | `id` of the group (required if the list is created for a group); | ||
|#

{% include [Footnote on parameters](../../../_includes/required.md) %}

## Example

```js
/* lists.section.delete */
var params = {
    'IBLOCK_TYPE_ID': 'lists',
    'IBLOCK_CODE': 'rest_1',
    'SECTION_CODE': 'Section_code_1'
};
BX24.callMethod(
    'lists.section.delete',
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

{% include [Footnote on examples](../../../_includes/examples.md) %}