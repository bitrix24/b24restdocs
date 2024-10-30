# Create or Update a Scrum Task tasks.api.scrum.task.update

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to Scrum

This method creates or updates a Scrum task. You will be able to:
- create a task in Scrum
- move a task from another project
- transfer it between the backlog and sprints
- change story points
- link an epic

A task must be created using the [tasks.task.add](../../../tasks/tasks-task-add.md) method or updated using the [tasks.task.update](../../../tasks/tasks-task-update.md) method. Linking the task to Scrum is specified in the group identifier parameter `GROUP_ID`. 

You can obtain the group identifier using the [create new group](../../sonet-group-create.md) method or the [get group list](../../socialnetwork-api-workgroup-list.md) method. A group is considered a Scrum if the `SCRUM_MASTER_ID` field is filled.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Task identifier ||
|| **fields***
[`object`](../../../data-types.md) | Object containing records about the Scrum task (detailed description provided [below](#parametr-fields)) in the following structure:

```js
fields: {
    entityId: 'value'
    storyPoints: 'value',
    epicId: 'value',
    sort: 'value'
}
```

||
|#

### Fields Parameter

#|
|| **Name**
`type` | **Description** ||
|| **entityId**
`integer` | Identifier of the backlog or sprint.

If the value is not specified, *Bitrix24* will automatically add the task to the Scrum backlog if it exists ||
|| **storyPoints**
`string` | Story Points — relative assessment of the task's complexity.

Can have a string value ||
|| **epicId**
`integer` | Epic identifier ||
|| **sort**
`integer` | Sorting ||
|#

## Code Examples

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"fields":{"epicId":1,"storyPoints":"8","entityId":2}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.api.scrum.task.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"fields":{"epicId":1,"storyPoints":"8","entityId":2},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.api.scrum.task.update
    ```

- JS

    ```js
    BX24.callMethod(
        'tasks.api.scrum.task.update',
        {
            id: 1,
            fields: 
            {
                epicId: 1,
                storyPoints: '8',
                entityId: 2
            }
        },
        function(res)
        {
            console.log(res);
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.api.scrum.task.update',
        [
            'id' => 1,
            'fields' => [
                'epicId' => 1,
                'storyPoints' => '8',
                'entityId' => 2
            ]
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
    "status" : "success",
    "data" : true,
    "errors" : []
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **status**
`string` | Response status.

Possible values:
- `success` 
- `error` 
||
|| **data**
`boolean`\|`null` | Returns:
- `true` — in case of success
- `null` — in case of error 
||
|| **errors**
`array` | Array of errors ||
|#  

## Error Handling

HTTP Status: **400**

```json
{
    "error": 0,
    "error_description": "Task not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Epic not found | Epic not found ||
|| `0` | Task not found | Task not found ||
|| `0` | Access denied | Access denied ||
|| `0` | Item not created | Task not added to Scrum ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./tasks-api-scrum-task-get.md)
- [{#T}](./tasks-api-scrum-task-get-fields.md)