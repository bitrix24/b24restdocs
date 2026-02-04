# Get Description of Storage Fields disk.storage.getfields

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.storage.getfields` returns the description of storage fields.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.storage.getfields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.storage.getfields
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'disk.storage.getfields',
            {}
        );
        
        const result = response.getData().result;
        console.log('Retrieved fields:', result);
        
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
                'disk.storage.getfields',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error retrieving fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.storage.getfields",
        {},
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
        'disk.storage.getfields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "ID": {
            "TYPE": "integer",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "NAME": {
            "TYPE": "string",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "CODE": {
            "TYPE": "string",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "MODULE_ID": {
            "TYPE": "string",
            "USE_IN_FILTER": false,
            "USE_IN_SHOW": true
        },
        "ENTITY_TYPE": {
            "TYPE": "string",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "ENTITY_ID": {
            "TYPE": "string",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "ROOT_OBJECT_ID": {
            "TYPE": "integer",
            "USE_IN_FILTER": false,
            "USE_IN_SHOW": true
        }
    },
    "time": {
        "start": 1769541038,
        "finish": 1769541038.113711,
        "duration": 0.11371111869812012,
        "processing": 0,
        "date_start": "2026-01-26T15:10:38+02:00",
        "date_finish": "2026-01-26T15:10:38+02:00",
        "operating_reset_at": 1769541638,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array with the description of storage fields.

The structure of the description for each field:
- `TYPE` — data type of the field
- `USE_IN_FILTER` — ability to use the field in filtering
- `USE_IN_SHOW` — availability of the field when receiving a response ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the storage ||
|| **NAME**
[`string`](../../data-types.md) | Name of the storage ||
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
[`string`](../../data-types.md) | Identifier of the object ||
|| **ROOT_OBJECT_ID**
[`integer`](../../data-types.md) | Identifier of the root folder of the storage ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-storage-add-folder.md)
- [{#T}](./disk-storage-get-children.md)
- [{#T}](./disk-storage-get-for-app.md)
- [{#T}](./disk-storage-get-list.md)
- [{#T}](./disk-storage-get-types.md)
- [{#T}](./disk-storage-get.md)
- [{#T}](./disk-storage-rename.md)
- [{#T}](./disk-storage-upload-file.md)