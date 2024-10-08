# List of Files and Folders in the Root of the Storage disk.storage.getchildren

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- no error response is provided

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.storage.getchildren` returns a list of files and folders that are directly in the root of the storage.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Identifier of the storage. ||
|| **filter**
[`unknown`](../../data-types.md) |  Optional parameter. Supports filtering by fields specified in [disk.folder.getfields](../folder/disk-folder-get-fields.md) as `USE_IN_FILTER: true`. ||
|#

{% note info %}

See also the description of [list methods](../../how-to-call-rest-api/list-methods-pecularities.md).

{% endnote %}

## Example

```js
BX24.callMethod(
    "disk.storage.getchildren",
    {
        id: 4,
        filter: {
            CREATED_BY: 1
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
{% include [Footnote on examples](../../../_includes/examples.md) %}

## Response on Success

> 200 OK

The response contains an array of objects, the structure of which is similar to [disk.folder.get](../folder/disk-folder-get.md), [disk.file.get](../file/disk-file-get.md).

```json
"result": [
{
    "ID": "13",
    "NAME": "near",
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
    "DETAIL_URL": "https://test.bitrix24.com/workgroups/group/3/disk/path/near/"
},
{
    "ID": "10",
    "NAME": "2511.jpg",
    "CODE": null,
    "STORAGE_ID": "4",
    "TYPE": "file",
    "PARENT_ID": "8",
    "DELETED_TYPE": "0",
    "CREATE_TIME": "2015-04-24T10:41:51+03:00",
    "UPDATE_TIME": "2015-04-24T15:52:43+03:00",
    "DELETE_TIME": null,
    "CREATED_BY": "1",
    "UPDATED_BY": "1",
    "DELETED_BY": "0",
    "DOWNLOAD_URL": "https://test.bitrix24.com/disk/downloadFile/10/?&ncc=1&filename=2511.jpg&auth=******",
    "DETAIL_URL": "https://test.bitrix24.com/workgroups/group/3/disk/file/2511.jpg"
},
{
    "ID": "11",
    "NAME": "549x700.png",
    "CODE": null,
    "STORAGE_ID": "4",
    "TYPE": "file",
    "PARENT_ID": "8",
    "DELETED_TYPE": "0",
    "CREATE_TIME": "2015-04-24T10:58:49+03:00",
    "UPDATE_TIME": "2015-04-24T12:01:32+03:00",
    "DELETE_TIME": null,
    "CREATED_BY": "1",
    "UPDATED_BY": "1",
    "DELETED_BY": "0",
    "DOWNLOAD_URL": "https://test.bitrix24.com/disk/downloadFile/11/?&ncc=1&filename=549x700.png&auth=******",
    "DETAIL_URL": "https://test.bitrix24.com/workgroups/group/3/disk/file/549x700.png"
}]
```