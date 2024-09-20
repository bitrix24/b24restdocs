# Get the List of Kanban / My Plan Stages task.stages.get

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: 
> - any user for My Plan stages
> - any user with access to the group for Kanban stages

This method retrieves the stages of Kanban / My Plan.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityId*** 
[`integer`](../../data-types.md) | Entity identifier. Possible values:
- `ID` of the group — the method will retrieve the Kanban stages of the group. An access error will be returned if the permission level is insufficient.
- `0` — the method will retrieve the stages of My Plan for the current user ||
|| **isAdmin** 
[`boolean`](../../data-types.md) | If set to `true`, permission checks will not be performed, provided that the requester is an account administrator ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "entityId": 0
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/task.stages.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: YOUR_ACCESS_TOKEN" \
    -d '{
    "entityId": 0
    }' \
    https://your-domain.bitrix24.com/rest/task.stages.get
    ```

- JS

    ```js
    const entityId = 0;
    BX24.callMethod(
        'task.stages.get',
        {
            entityId: entityId,
        },
        function(res)
        {
            console.log(res);
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php'); // include CRest PHP SDK

    $entityId = 0;

    // execute request to REST API
    $result = CRest::call(
        'task.stages.get',
        [
            'entityId' => $entityId
        ]
    );

    // Process the response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "5": {
         "ID": "5",
         "TITLE": "Not Planned",
         "SORT": "100",
         "COLOR": "00C4FB",
         "SYSTEM_TYPE": "NEW",
         "ENTITY_ID": "1",
         "ENTITY_TYPE": "U",
         "ADDITIONAL_FILTER": [],
         "TO_UPDATE": [],
         "TO_UPDATE_ACCESS": null
        },
        "6": {
         "ID": "6",
         "TITLE": "Will Do This Week",
         "SORT": "200",
         "COLOR": "47D1E2",
         "SYSTEM_TYPE": null,
         "ENTITY_ID": "1",
         "ENTITY_TYPE": "U",
         "ADDITIONAL_FILTER": [],
         "TO_UPDATE": [],
         "TO_UPDATE_ACCESS": null
        }
    }
}
```

## Returned Data

#|
|| **Field**
`type` | **Description** ||
|| **result** 
`object` | An object containing data about the Kanban / My Plan stages, with stage identifiers as keys ||
|| **ID** 
`integer` | Stage identifier ||
|| **TITLE** 
`string` | Name ||
|| **SORT** 
`integer` | Sorting ||
|| **COLOR** 
`string` | Color in RGB format ||
|| **SYSTEM_TYPE** 
`string` | System type (e.g., `NEW`, `PROGRESS`, `WORK`, `REVIEW`, `FINISH`) ||
|| **ENTITY_ID** 
`integer` | Entity identifier (group or user) ||
|| **ENTITY_TYPE** 
`string` | Entity type (e.g., `U` for user, `G` for group) ||
|| **ADDITIONAL_FILTER** 
`array` | Additional filters. 

System parameter. Always has the value of an empty array ||
|| **TO_UPDATE** 
`array` | Array of items to update.

System parameter. Always has the value of an empty array ||
|| **TO_UPDATE_ACCESS** 
`null` | Functions applied to the task when moving to this stage.

System parameter. Always has the value `null` ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "You cannot view stages in this group"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Value** ||
|| `ACCESS_DENIED` | You cannot view stages in this group ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./task-stages-add.md)
- [{#T}](./task-stages-update.md)
- [{#T}](./task-stages-can-move-task.md)
- [{#T}](./task-stages-move-task.md)
- [{#T}](./task-stages-delete.md)