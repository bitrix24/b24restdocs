# Create a section of the universal list lists.section.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

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

The method `lists.section.add` creates a list section. If the section is created successfully, the response is `true`, otherwise *Exception*.

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
|| **SOCNET_GROUP_ID**^*^
[`unknown`](../../data-types.md) | Group ID (required if the list is created for a group); | ||
|| **IBLOCK_SECTION_ID**
[`unknown`](../../data-types.md) | Identifier of the parent section; if not specified, the section will be root | ||
|| **FIELDS**
[`unknown`](../../data-types.md) | Array of fields and values. Required field: NAME. | ||
|| **SECTION_CODE**^*^
[`unknown`](../../data-types.md) | Symbolic code of the section (required). | ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    /* lists.section.add */
    var params = {
        'IBLOCK_TYPE_ID': 'lists',
        'IBLOCK_CODE': 'rest_1',
        'SECTION_CODE': 'Section_code_1',
        'FIELDS': {
            'NAME': 'Section_1',
        }
    };
    BX24.callMethod(
        'lists.section.add',
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

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}