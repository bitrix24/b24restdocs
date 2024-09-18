# Add a Kanban / My Plan Stage task.stages.add

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method:
> - any user for My Plan stages
> - any user with access to the group for Kanban stages

This method adds a Kanban / My Plan stage.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values (detailed description provided [below](#parametr-fields)) for adding a new stage ||
|| **isAdmin**
[`boolean`](../../data-types.md) | If set to `true`, permission checks will not occur, provided the requester is an account administrator ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TITLE*** [`string`](../../data-types.md) | Stage title ||
|| **COLOR** [`string`](../../data-types.md) | Stage color in RGB format ||
|| **AFTER_ID** [`integer`](../../data-types.md) | Identifier of the stage after which the new stage should be added.

If not specified or equal to `0`, it will be added at the beginning ||
|| **ENTITY_ID** [`integer`](../../data-types.md)| Identifier of the entity.

Can equal the `ID` of the group, in which case the stage will be added to the group's Kanban.

If equal to `0` or absent, the stage is added to the current user's My Plan.

If the permission level is insufficient, an access error will be displayed ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "fields": {
        "TITLE": "Stage Title",
        "COLOR": "#FFAAEE",
        "AFTER_ID": 1,
        "ENTITY_ID": 1
    },
    "isAdmin": false
    }' \
    https://your-domain.com/rest/_USER_ID_/_CODE_/task.stages.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: YOUR_ACCESS_TOKEN" \
    -d '{
    "fields": {
        "TITLE": "Stage Title",
        "COLOR": "#FFAAEE",
        "AFTER_ID": 1,
        "ENTITY_ID": 1
    },
    "isAdmin": false
    }' \
    https://your-domain.com/rest/task.stages.add
    ```

- JS

    ```js
    BX24.callMethod(
        'task.stages.add',
        {
            fields: {
                TITLE: 'Stage Title',
                COLOR: '#FFAAEE',
                AFTER_ID: 1,
                ENTITY_ID: 1
            },
            isAdmin: false,
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php'); // include CRest PHP SDK

    $fields = [
        "TITLE" => "Stage Title",
        "COLOR" => "#FFAAEE",
        "AFTER_ID" => 1,
        "ENTITY_ID" => 1
    ];

    // execute request to REST API
    $result = CRest::call(
        'task.stages.add',
        [
            'fields' => $fields,
            'isAdmin' => false
        ]
    );

    // Handle response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": 1
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result** 
[`integer`](../../data-types.md) | Identifier of the added stage ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "EMPTY_TITLE",
    "error_description": "Stage title is not specified"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `EMPTY_TITLE` | Stage title is not specified ||
|| `ACCESS_DENIED` | You cannot manage stages ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./task-stages-update.md)
- [{#T}](./task-stages-get.md)
- [{#T}](./task-stages-can-move-task.md)
- [{#T}](./task-stages-move-task.md)
- [{#T}](./task-stages-delete.md)