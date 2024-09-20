# Update Kanban Stage / My Plan task.stages.update

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method:
> - any user for My Plan stages
> - any user with group access for Kanban stages

This method updates the stages of the Kanban / My Plan.

The method can also be used to move a stage from one position to another — simply provide the desired `AFTER_ID`.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the stage ||
|| **fields***
[`array`](../../data-types.md) | Field values (detailed description provided [below](#parametr-fields)) for updating the Kanban / My Plan stage ||
|| **isAdmin**
[`boolean`](../../data-types.md) | If set to `true`, permission checks will not occur, provided the requester is an account administrator ||
|#

### Parameter fields

#|
|| **Name**
`type` | **Description** ||
|| **TITLE** [`string`](../../data-types.md) | Title of the stage ||
|| **COLOR** [`string`](../../data-types.md) | Color of the stage in RGB format ||
|| **AFTER_ID** [`integer`](../../data-types.md) | Identifier of the stage after which the new stage should be added.

If not specified or equal to `0`, it will be added at the beginning ||
|#

When updating a group stage with insufficient permission level, an access error will be displayed.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "id": 5,
    "fields": {
        "TITLE": "New Stage",
        "SORT": 200,
        "COLOR": "FF5733"
    }
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/task.stages.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: YOUR_ACCESS_TOKEN" \
    -d '{
    "id": 5,
    "fields": {
        "TITLE": "New Stage",
        "SORT": 200,
        "COLOR": "FF5733"
    }
    }' \
    https://your-domain.bitrix24.com/rest/task.stages.update
    ```

- JS

    ```js
    const stageId = 5;
    const fields = {
        TITLE: "New Stage",
        SORT: 200,
        COLOR: "FF5733"
    };
    BX24.callMethod(
        'task.stages.update',
        {
            id: stageId,
            fields: fields
        },
        function(res)
        {
            console.log(res);
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    $stageId = 5;
    $fields = [
        "TITLE" => "New Stage",
        "SORT" => 200,
        "COLOR" => "FF5733"
    ];

    // executing request to REST API
    $result = CRest::call(
        'task.stages.update',
        [
            'id' => $stageId,
            'fields' => $fields
        ]
    );

    // Handling response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: 200

```json
{
    "result": true
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result** 
[`boolean`](../../data-types.md) | Returns `true` if the stage was successfully updated
||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "You cannot modify stages in this group"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | You cannot modify stages in this group ||
|| `NOT_FOUND` | Stage not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./task-stages-add.md)
- [{#T}](./task-stages-get.md)
- [{#T}](./task-stages-can-move-task.md)
- [{#T}](./task-stages-move-task.md)
- [{#T}](./task-stages-delete.md)