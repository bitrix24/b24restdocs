# Update List Element lists.element.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent
- links to pages that are not yet created are not specified

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `lists.element.update` updates a list element. If the element is successfully updated, the response is `true`, otherwise *Exception*.

{% note warning %}

All fields of the element and their values must be passed in the request.

{% endnote %}

To upload files to a File (Disk) type field, you need to:

1. use the REST API of the disk module: disk.folder.uploadfile and disk.storage.uploadfile. In the response when uploading these files, you will receive `"ID": 290`.
2. Get the list of `IDs` of the uploaded files.
3. Then, using the REST API of the lists module, add the files to the required field. If the field already has attached files, you need to retrieve the previous values from [lists.element.get](./lists-element-get.md) and pass them along with the new ones:

```js
var params = {
    'IBLOCK_TYPE_ID': 'lists',
    'IBLOCK_ID': '41',
    'ELEMENT_CODE': 'element1',
    'FIELDS': {
        'NAME': 'Test element 1',
        'PROPERTY_121': {'4754': ['50', 'n1582']} // or without id 'PROPERTY_121': {'n0': ['50', 'n1582']}
    }
};
BX24.callMethod(
    'lists.element.update',
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
Values in the File (Disk) field without the prefix `"n"` are already attached files (attachedId), while those with the prefix are your new files that have been previously uploaded to the disk.

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
|| **ELEMENT_CODE/ELEMENT_ID**^*^
[`unknown`](../../data-types.md) | code or `id` of the element (required) ||
|| **FIELDS**
[`unknown`](../../data-types.md) | array of fields and values ||
|| **SOCNET_GROUP_ID**^*^
[`unknown`](../../data-types.md) | `id` of the group (required if the list is created for a group); ||
|#

{% include [Parameter Footnote](../../../_includes/required.md) %}

## Example

```javascript
var params = {
    'IBLOCK_TYPE_ID': 'lists_socnet',
    'IBLOCK_CODE': 'rest_1',
    'ELEMENT_CODE': 'element_1',
    'FIELDS': {
        'NAME': 'Test element (Update)',
        'PROPERTY_62': {
        '599': 'Text string (Update)'
        },
        'PROPERTY_63': {
        '600': '73',
        '601': '97',
        '602': '17'
        }
    }
};
BX24.callMethod(
    'lists.element.update',
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

{% include [Example Footnote](../../../_includes/examples.md) %}