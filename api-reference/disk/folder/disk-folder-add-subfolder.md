# Create Subfolder disk.folder.addsubfolder

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- detailed description of the data parameter is missing
- examples are absent (there should be three examples - curl, js, php)
- response in case of an error is missing

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.folder.addsubfolder` creates a subfolder.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Identifier of the folder. ||
|| **data**
[`unknown`](../../data-types.md) | Array describing the folder. The required field `NAME` — the name of the new folder. ||
|#

## Example

```js
BX24.callMethod(
    "disk.folder.addsubfolder",
    {
        id: 8,
        data: {
            NAME: 'New sub folder'
        }
    },
    function (result)
    {
        if (result.error())
            console.error(result.error());
        else
            console.dir(result.data());
    }
);
```
{% include [Examples Note](../../../_includes/examples.md) %}

## Response on Success

> 200 OK

The response has the same structure as in [disk.folder.get](./disk-folder-get.md).

```json
"result":{
    "ID": "13",
    "NAME": "New sub folder",
    "CODE": null,
    "STORAGE_ID": "4",
    "TYPE": "folder",
    "PARENT_ID": "8",
    "DELETED_TYPE": "0",
    "CREATE_TIME": "2015-04-24T12:39:35+03:00",
    "UPDATE_TIME": "2015-04-24T12:39:35+03:00",
    "DELETE_TIME": null,
    "CREATED_BY": "1",
    "UPDATED_BY": "1",
    "DELETED_BY": "0",
    "DETAIL_URL": "https://test.bitrix24.com/workgroups/group/3/disk/path/New/"
}
```