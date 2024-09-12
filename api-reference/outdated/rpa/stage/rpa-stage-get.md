# Get Information About Stage rpa.stage.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.stage.get` returns information about a stage by its ID.

#|
|| **Parameter** / **Type** | **Description** ||
|| **id**^*^ 
[`number`](../../../data-types.md) | Stage ID. ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}

## Successful Response

> 200 OK

```json
{
    "id": 1,
    "name": "Launch",
    "code": "",
    "color": "22B9FF",
    "sort": 1000,
    "semantic": null,
    "typeId": 1,
    "isFirst": true,
    "isSuccess": false,
    "isFail": false,
    "tasks": [
        {
            "title": "Task",
            "robotType": "RpaApproveActivity",
            "robotName": "A43555_78925_98855_46118",
            "canAppendResponsibles": true,
            "users": [
                {
                    "id": "U1",
                    "entityId": 1,
                    "name": "Anton",
                    "photoSrc": "",
                    "url": "\/company\/personal\/user\/1\/",
                    "entityType": "users"
                },
                {
                    "id": "U4",
                    "entityId": 4,
                    "name": "Peter",
                    "photoSrc": "",
                    "url": "\/company\/personal\/user\/4\/",
                    "entityType": "users"
                }
            ]
        }
    ],
    "robotsCount": 0,
    "possibleNextStages": [1,2,3,4,5],
    "permissions": {
        "droppable": true,
        "canMoveFrom": true
    }
}
```

- `name` - name of the stage
- `code` - symbolic code. Can be used as an external identifier
- `color` - HEX color code of the stage, 6 characters
- `sort` - sorting index
- `semantic` - semantic code of the stage. Can be either `SUCCESS` or `FAIL`
- `typeId` - process identifier
- `isFirst` - computed field. `true` if this is the first stage of the process
- `isSuccess` - computed field. `true` if this stage is successful
- `isFail` - computed field. `true` if this stage is a failure
- `tasks` - array of tasks for the stage. Each entry has the following structure:
- `title` - title of the task
- `robotType` - type of the task. Can take one of the following values:
    - `RpaApproveActivity` - approve or reject
    - `RpaMoveActivity` - simply move
    - `RpaRequestActivity` - request information
    - `RpaReviewActivity` - review information
- `robotName` - name of the activity
- `users` - array of participants in the task (for rendering in the kanban at the stage)
- `robotsCount` - computed field. Number of robots at the stage
- `possibleNextStages` - array of stage identifiers to which the item can be moved. Not used
- `permissions` - set of permissions (for kanban).
- `droppable` - items can be moved to this stage
- `canMoveFrom` - items can be moved from this stage