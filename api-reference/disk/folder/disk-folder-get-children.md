# Get a list of files and folders in the folder disk.folder.getchildren

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is absent

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.folder.getchildren` returns a list of files and folders that are directly in the folder.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Identifier of the folder. ||
|| **filter**
[`unknown`](../../data-types.md) |  Optional parameter. Supports filtering by fields specified in [disk.folder.getfields](./disk-folder-get-fields.md) as `USE_IN_FILTER: true`. ||
|| **START** | The ordinal number of the list item from which to return the next items when calling the current method. Details in the article [{#T}](../../how-to-call-rest-api/list-methods-pecularities.md) ||
|#

{% note info %}

See also the description of [list methods](../../how-to-call-rest-api/list-methods-pecularities.md).

{% endnote %}

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "disk.folder.getchildren",
        {
            id: 8,
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

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response in case of success

> 200 OK

The response is an array of objects, the structure of which is similar to [disk.folder.get](./disk-folder-get.md), [disk.file.get](../file/disk-file-get.md).

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
    "CREATE_TIME": "2015-04-24T12:39:35+02:00",
    "UPDATE_TIME": "2015-04-24T12:39:35+02:00",
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
    "CREATE_TIME": "2015-04-24T10:41:51+02:00",
    "UPDATE_TIME": "2015-04-24T15:52:43+02:00",
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
    "CREATE_TIME": "2015-04-24T10:58:49+02:00",
    "UPDATE_TIME": "2015-04-24T12:01:32+02:00",
    "DELETE_TIME": null,
    "CREATED_BY": "1",
    "UPDATED_BY": "1",
    "DELETED_BY": "0",
    "DOWNLOAD_URL": "https://test.bitrix24.com/disk/downloadFile/11/?&ncc=1&filename=549x700.png&auth=******",
    "DETAIL_URL": "https://test.bitrix24.com/workgroups/group/3/disk/file/549x700.png"
}]
```