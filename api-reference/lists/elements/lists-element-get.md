# Get Parameters of an Element or List of Elements lists.element.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `lists.element.get` returns a list of elements or a single element. On success, it returns data for the element(s); otherwise, it returns an empty array.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **IBLOCK_TYPE_ID**
[`unknown`](../../data-types.md) | `id` of the information block type (required):
- **lists** - list information block type
- **bitrix_processes** - processes information block type
- **lists_socnet** - group lists information block type ||
|| **SOCNET_GROUP_ID**
[`unknown`](../../data-types.md) | `id` of the group (required if the list is created for a group); ||
|| **IBLOCK_CODE/IBLOCK_ID**
[`unknown`](../../data-types.md) | code or `id` of the information block (required) ||
|| **ELEMENT_CODE/ELEMENT_ID**
[`unknown`](../../data-types.md) | code or `id` of the element (If not specified, it will return a list of all elements in the list) ||
|| **ELEMENT_ORDER**
[`unknown`](../../data-types.md) | Sorting. An array of fields of the information block elements. Sorting direction: **asc** (ascending) or **desc** (descending)

{% include [Parameter Note](../../../_includes/required.md) %}

Example:
```js
'ELEMENT_ORDER': { "ID": "DESC" }
```

Sorting of all multiple properties is not supported, as well as properties:

S:Money, PREVIEW_TEXT, DETAIL_TEXT, S:ECrm, S:map_yandex, PREVIEW_PICTURE, DETAIL_PICTURE, S:DiskFile, IBLOCK_SECTION_ID, BIZPROC, COMMENTS. ||

|| **FILTER** | Filtering elements. An object with a list of fields and values.
Almost all fields from the filter CIBlockElement::GetList are available for filtering. For example, to filter by a numeric field, you need to specify the equal sign:
```js
'FILTER': {
    '=ID': [120,121],
}
```
Full-text search is also available. To do this, you need to use the SEARCHABLE_CONTENT field with the prefix "*"; ||
|#

## Examples

```js
var params = {
    'IBLOCK_TYPE_ID': 'lists_socnet',
    'IBLOCK_CODE': 'rest_1',
    'ELEMENT_CODE': 'element_1'
};
BX24.callMethod(
    'lists.element.get',
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

```js
var params = {
    'IBLOCK_TYPE_ID': 'lists',
    'IBLOCK_ID': '41',
    'FILTER': {
        '>=DATE_CREATE': '03/27/2018 00:00:00',
        '<=DATE_CREATE': '03/27/2018 23:59:59',
    }
};
```

{% include [Examples Note](../../../_includes/examples.md) %}