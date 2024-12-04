# Get file parameters by identifier disk.file.get

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

The method `disk.file.get` returns a file by its identifier.

{% note warning %}

The link to download the file from the `DOWNLOAD_URL` parameter contains an authorization token and is intended for downloading the file on behalf of the application. This link should not be "shared" or used for public interfaces.

{% endnote %}

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | File identifier. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "disk.file.get",
        {
            id: 10
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

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Response on success

> 200 OK

```json
"result": {
    "ID": "10", //identifier
    "NAME": "2511.jpg", //file name
    "CODE": null, //symbolic code
    "STORAGE_ID": "4", //storage identifier
    "TYPE": "file",
    "PARENT_ID": "8", //parent folder identifier
    "DELETED_TYPE": "0", //deletion marker
    "CREATE_TIME": "2015-04-24T10:41:51+02:00", //creation time
    "UPDATE_TIME": "2015-04-24T15:52:43+02:00", //modification time
    "DELETE_TIME": null, //time moved to trash
    "CREATED_BY": "1", //identifier of the user who created the file
    "UPDATED_BY": "1", //identifier of the user who modified the file
    "DELETED_BY": "0", //identifier of the user who moved the file to trash
    "DOWNLOAD_URL": "https://test.bitrix24.com/disk/downloadFile/10/?&ncc=1&filename=2511.jpg&auth=******",
//returns url for downloading the file by the application
    "DETAIL_URL": "https://test.bitrix24.com/workgroups/group/3/disk/file/2511.jpg"
//link to the detailed information page about the file
}
```