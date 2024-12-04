# Get Data from the Infoblock lists.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for standard writing
- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `lists.get` method returns data from the infoblock. On success, it will return the infoblock data; otherwise, an empty array will be returned. This method allows you to retrieve a list of data from all infoblocks of the specified type. When working with social network groups, it is essential to specify the `ID` of the group; otherwise, an access error will occur.

## Parameters
#|
|| **Parameter** | **Description** ||
|| **IBLOCK_TYPE_ID**^*^
[`unknown`](../../data-types.md) | `id` of the infoblock type (required):
- **lists** - infoblock type for lists
- **bitrix_processes** - infoblock type for processes
- **lists_socnet** - infoblock type for group lists ||
|| **IBLOCK_CODE/IBLOCK_ID**
[`unknown`](../../data-types.md) | code or `id` of the infoblock (if not specified, data for all lists of the specified infoblock type will be returned) ||
|| **SOCNET_GROUP_ID**
[`unknown`](../../data-types.md) | `id` of the group, required if the list is in groups. ||
|| **IBLOCK_ORDER**
[`unknown`](../../data-types.md) | Sorting. An array of fields for sorting the infoblock sections. Sorting direction: **asc** (ascending) or **desc** (descending). Example: 
`'IBLOCK_ORDER': { "ID": "DESC" }` ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    var params = {
        'IBLOCK_TYPE_ID': 'lists',
        'IBLOCK_CODE': 'rest_1'
    };
    BX24.callMethod(
        'lists.get',
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

{% include [Example Notes](../../../_includes/examples.md) %}