# Get Parameters of the Section or List of Sections `lists.section.get`

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `lists.section.get` returns a list of sections or a section. On success, it returns data for the section(s); otherwise, it returns an empty array.

#|
|| **Parameter** | **Description** ||
|| **IBLOCK_TYPE_ID**^*^
[`unknown`](../../data-types.md) | Identifier of the information block type (required). Possible values: 
- lists - list information block type 
- bitrix_processes - processes information block type 
- lists_socnet - group lists information block type | ||
|| **IBLOCK_CODE/IBLOCK_ID**^*^
[`unknown`](../../data-types.md) | Code or identifier of the information block (required). | ||
|| **FILTER**
[`unknown`](../../data-types.md) | Array of fields and values for filtering. Fields available for filtering are from the `CIBlockSection::GetList` filter | ||
|| **SELECT**
[`unknown`](../../data-types.md) | Array of fields to select. Available fields are described in the `CIBlockSection::GetList` documentation | ||
|| **SOCNET_GROUP_ID**^*^
[`unknown`](../../data-types.md) | Group identifier (required if the list is created for a group) | ||
|#

{% include [Footnote on parameters](../../../_includes/required.md) %}

(**) - except for user-defined (UF_) fields.

## Example

{% list tabs %}

- JS

    ```js
    /* lists.section.get */
    var params = {
        'IBLOCK_TYPE_ID': 'lists',
        'IBLOCK_CODE': 'rest_1',
        'FILTER': {
            'NAME': 'section_%'
        },
        'SELECT': ['ID', 'NAME']
    };
    BX24.callMethod(
        'lists.section.get',
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

{% include [Footnote on examples](../../../_includes/examples.md) %}