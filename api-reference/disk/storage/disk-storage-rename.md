# Rename Application Storage disk.storage.rename

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `disk.storage.rename` renames the application storage.

{% note info "" %}

The method works only in the context of the [application](../../../settings/app-installation/index.md)

{% endnote %}

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the application storage.

The identifier can be obtained using the method [disk.storage.getforapp](./disk-storage-get-for-app.md) || 
|| **newName***
[`string`](../../data-types.md) | New name for the storage ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1366,"newName":"Bitrix REST API","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.storage.rename
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.storage.rename',
            {
                id: 1366,
                newName: 'Bitrix REST API'
            }
        );
        
        const result = response.getData().result;
        console.log('Renamed storage:', result);
        
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
                'disk.storage.rename',
                [
                    'id' => 1366,
                    'newName' => 'Bitrix REST API'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error renaming storage: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.storage.rename",
        {
            id: 1366,
            newName: 'Bitrix REST API'
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
        'disk.storage.rename',
        [
            'id' => 1366,
            'newName' => 'Bitrix REST API'
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
    "result": {
        "ID": "1366",
        "NAME": "Bitrix REST API",
        "CODE": null,
        "MODULE_ID": "disk",
        "ENTITY_TYPE": "restapp",
        "ENTITY_ID": "3",
        "ROOT_OBJECT_ID": "8910"
    },
    "time": {
        "start": 1770048169,
        "finish": 1770048169.935598,
        "duration": 0.9355978965759277,
        "processing": 0,
        "date_start": "2026-02-02T14:02:49+01:00",
        "date_finish": "2026-02-02T14:02:49+01:00",
        "operating_reset_at": 1770048769,
        "operating": 0.11735081672668457
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array with the description of the storage fields ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the storage ||
|| **NAME**
[`string`](../../data-types.md) | Name of the storage ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the storage ||
|| **MODULE_ID**
[`string`](../../data-types.md) | Identifier of the module to which the storage belongs ||
|| **ENTITY_TYPE**
[`string`](../../data-types.md) | Type of the object associated with the storage ||
|| **ENTITY_ID**
[`string`](../../data-types.md) | Identifier of the object associated with the storage ||
|| **ROOT_OBJECT_ID**
[`integer`](../../data-types.md) | Identifier of the root folder of the storage ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"ERROR_NOT_FOUND",
    "error_description":"Could not find entity with id `X`"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %} 

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | Storage with the specified `id` not found ||
|| — | Access denied (invalid type of storage) | Storage not associated with the application ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to rename the storage ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-storage-add-folder.md)
- [{#T}](./disk-storage-get-children.md)
- [{#T}](./disk-storage-get-fields.md)
- [{#T}](./disk-storage-get-for-app.md)
- [{#T}](./disk-storage-get-list.md)
- [{#T}](./disk-storage-get-types.md)
- [{#T}](./disk-storage-get.md)
- [{#T}](./disk-storage-upload-file.md)