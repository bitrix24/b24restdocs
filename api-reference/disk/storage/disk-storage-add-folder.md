# Create a folder in the root of the storage disk.storage.addfolder

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- detailed description of the data parameter is missing
- examples are absent (there should be three examples - curl, js, php)
- response in case of error is missing

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.storage.addfolder` creates a folder in the root of the storage.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Identifier of the storage. ||
|| **data**
[`unknown`](../../data-types.md) | Array describing the folder. The required field `NAME` — the name of the new folder. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "disk.storage.addfolder",
        {
            id: 4,
            data: {'NAME': 'New'}
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

## Response in case of success

> 200 OK

The returned structure is similar to that provided in [disk.folder.get](../folder/disk-folder-get.md).

```json
"result": {
    "ID": "13",
    "NAME": "New",
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
    "DETAIL_URL": "https://test.bitrix24.com/workgroups/group/3/disk/path/New/"
}
```