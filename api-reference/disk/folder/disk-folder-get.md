# Get Folder Parameters by ID disk.folder.get

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.folder.get` returns a folder by its ID.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Folder ID. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "disk.folder.get",
        {
            id: 8
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

## Response on Success

> 200 OK

```json
"result": {
    "ID": "8", //identifier
    "NAME": "newfolder", //folder name
    "CODE": null, //symbolic code
    "STORAGE_ID": "4", //storage identifier
    "TYPE": "folder",
    "PARENT_ID": "12", //parent folder identifier
    "DELETED_TYPE": "0", //deletion marker
    "CREATE_TIME": "2015-04-24T10:41:51+02:00", //creation time
    "UPDATE_TIME": "2015-04-24T15:52:43+02:00", //modification time
    "DELETE_TIME": null, //time moved to trash
    "CREATED_BY": "1", //identifier of the user who created the file
    "UPDATED_BY": "1", //identifier of the user who modified the file
    "DELETED_BY": "0", //identifier of the user who moved the file to trash
    "DETAIL_URL": "https://test.bitrix24.com/workgroups/group/3/disk/path/newfolder" //link to view the folder's file list
}
```