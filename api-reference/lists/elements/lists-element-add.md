# Create a List Element lists.element.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent
- links to pages that have not yet been created are not provided

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `lists.element.add` creates a list element. If the element is created successfully, the response is `true`; otherwise, it returns *Exception*.

To upload files to a File (Disk) type field, you need to:

1. use the REST API of the disk module: disk.folder.uploadfile and disk.storage.uploadfile. In the response when uploading these files, you will receive `"FILE_ID": 290`.
2. Get the list of uploaded file `IDs`.
3. Then, using the REST API of the lists module, add the files to the desired field:

```js
var params = {
    'IBLOCK_TYPE_ID': 'lists',
    'IBLOCK_ID': '41',
    'ELEMENT_CODE': 'element1',
    'FIELDS': {
        'NAME': 'Test element 1',
        'PROPERTY_121': { 'n0':["n1582"]}
    }
};
BX24.callMethod(
    'lists.element.add',
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
|| **ELEMENT_CODE**^*^
[`unknown`](../../data-types.md) | code of the information block element (required) ||
|| **LIST_ELEMENT_URL**
[`unknown`](../../data-types.md) | template address to list elements ||
|| **FIELDS**
[`unknown`](../../data-types.md) | array of fields and values ||
|| **SOCNET_GROUP_ID**^*^
[`unknown`](../../data-types.md) | `id` of the group (required if the list is created for a group); ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

```js
var params = {
    'IBLOCK_TYPE_ID': 'lists_socnet',
    'IBLOCK_CODE': 'rest_1',
    'ELEMENT_CODE': 'element_1',
    'LIST_ELEMENT_URL': '#list_id#/element/#section_id#/#element_id#/',
    'FIELDS': {
        'NAME': 'Test element',
        'PROPERTY_62': 'Text string',
        'PROPERTY_63': {
            '0': '7',
            '1': '9',
            '2': '10'
        }
    }
};
BX24.callMethod(
    'lists.element.add',
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

Example of adding a file:

```js
var params = {
    'IBLOCK_TYPE_ID': 'lists',
    'IBLOCK_ID': '41',
    'ELEMENT_CODE': 'element1',
    'FIELDS': {
        'NAME': 'Test element 1',
        'PROPERTY_122': document.getElementById('fileInputId') // PROPERTY_122 - Custom property of type "File"
    }
};
BX24.callMethod(
    'lists.element.add',
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

{% include [Example Note](../../../_includes/examples.md) %}