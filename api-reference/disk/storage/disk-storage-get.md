# Get Storage Description disk.storage.get

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Read" access permission for the required storage

The method `disk.storage.get` returns storage data.

## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Storage identifier.

The identifier can be obtained using the method [disk.storage.getlist](./disk-storage-get-list.md) ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1357}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.storage.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1357,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.storage.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.storage.get',
            {
                id: 1357
            }
        );
        
        const result = response.getData().result;
        console.log('Retrieved storage:', result);
        
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
                'disk.storage.get',
                [
                    'id' => 1357
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error retrieving storage: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.storage.get",
        {
            id: 1357
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
        'disk.storage.get',
        [
            'id' => 1357
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
        "ID": "1357",
        "NAME": "Storage",
        "CODE": null,
        "MODULE_ID": "disk",
        "ENTITY_TYPE": "user",
        "ENTITY_ID": "1269",
        "ROOT_OBJECT_ID": "8875"
    },
    "time": {
        "start": 1769545048,
        "finish": 1769545048.556574,
        "duration": 0.5565741062164307,
        "processing": 0,
        "date_start": "2026-01-26T16:37:28+02:00",
        "date_finish": "2026-01-26T16:37:28+02:00",
        "operating_reset_at": 1769545648,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array with storage data.

Returns `null` if `id` is not a number ||
|| **ID**
[`integer`](../../data-types.md) | Storage identifier ||
|| **NAME**
[`string`](../../data-types.md) | Storage name ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the storage ||
|| **MODULE_ID**
[`string`](../../data-types.md) | Identifier of the module to which the storage belongs ||
|| **ENTITY_TYPE**
[`string`](../../data-types.md) | Type of the object associated with the storage.

Possible values:
- `user` — user storage
- `common` — common documents storage
- `group` — group storage  ||
|| **ENTITY_ID**
[`string`](../../data-types.md) | Object identifier ||
|| **ROOT_OBJECT_ID**
[`integer`](../../data-types.md) | Identifier of the root folder of the storage ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"ERROR_ARGUMENT",
    "error_description":"Invalid value of parameter {Parameter #0}"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | Invalid value of parameter {Parameter #0} | Required parameter `id` is not specified ||
|| `ERROR_NOT_FOUND` | Could not find entity with id `X` | Storage with the specified `id` not found ||
|| `ACCESS_DENIED` | Access denied | Insufficient rights to read the storage ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-storage-add-folder.md)
- [{#T}](./disk-storage-get-fields.md)
- [{#T}](./disk-storage-get-children.md)
- [{#T}](./disk-storage-get-for-app.md)
- [{#T}](./disk-storage-get-list.md)
- [{#T}](./disk-storage-get-types.md)
- [{#T}](./disk-storage-rename.md)
- [{#T}](./disk-storage-upload-file.md)