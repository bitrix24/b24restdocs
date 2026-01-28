# Assign Access Permissions to Folder disk.folder.sharetouser

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Share" permission for the specified folder

The method `disk.folder.sharetouser` assigns access permissions to a folder.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the folder.

The identifier can be obtained using the method [disk.storage.getchildren](../storage/disk-storage-get-children.md) if the folder is located at the root of the storage, and using the method [disk.folder.getchildren](./disk-folder-get-children.md) if the folder is located within another folder ||
|| **userId***
[`integer`](../../data-types.md) | Identifier of the user to whom access is granted.

The identifier can be obtained using the method [user.get](../../user/user-get.md) ||
|| **taskName***
[`string`](../../data-types.md) | Level of access granted to the user. Possible values:

- `disk_access_read` — read
- `disk_access_add` — add
- `disk_access_edit` — edit
- `disk_access_full` — full access ||
|#

{% note info "" %}

The current user cannot grant permissions higher than their own level. For example, if the current user only has "Read" permission for the specified folder, they will not be able to grant another user "Edit" or "Full access" permissions.

{% endnote %} 

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8994,"userId":1271,"taskName":"disk_access_read"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.folder.sharetouser
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8994,"userId":1271,"taskName":"disk_access_read","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.folder.sharetouser
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.folder.sharetouser',
            {
                id: 8994,
                userId: 1271,
                taskName: 'disk_access_read',
            }
        );
        
        const result = response.getData().result;
        console.log('Folder shared to user with ID:', result);
        
        processResult(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'disk.folder.sharetouser',
                [
                    'id' => 8994,
                    'userId' => 1271,
                    'taskName' => 'disk_access_read'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error sharing folder to user: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.folder.sharetouser",
        {
            id: 8994,              
            userId: 1271,           
            taskName: 'disk_access_read'
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'disk.folder.sharetouser',
        [
            'id' => 8994,
            'userId' => 1271,
            'taskName' => 'disk_access_read'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1768921227,
        "finish": 1768921228.02202,
        "duration": 1.0220201015472412,
        "processing": 1,
        "date_start": "2026-01-20T17:00:27+02:00",
        "date_finish": "2026-01-20T17:00:28+02:00",
        "operating_reset_at": 1768921827,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if permissions are successfully assigned ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"ERROR_ARGUMENT",
    "error_description":"Invalid value of parameter {Parameter #1}"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | Invalid value of parameter {Parameter #1} | Required parameter not specified ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | Folder with the specified `id` not found ||
|| `ACCESS_DENIED` | Access denied | Attempt to set permission level higher than the current user's ||
|| `ACCESS_DENIED` | Access denied | Incorrect value provided for parameter `taskName` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-folder-add-subfolder.md)
- [{#T}](./disk-folder-copy-to.md)
- [{#T}](./disk-folder-delete-tree.md)
- [{#T}](./disk-folder-get-children.md)
- [{#T}](./disk-folder-get-external-link.md)
- [{#T}](./disk-folder-get-fields.md)
- [{#T}](./disk-folder-get.md)
- [{#T}](./disk-folder-move-to.md)
- [{#T}](./disk-folder-rename.md)
- [{#T}](./disk-folder-restore.md)
- [{#T}](./disk-folder-upload-file.md)