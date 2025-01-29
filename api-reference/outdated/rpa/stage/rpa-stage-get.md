# Get Information About Stage rpa.stage.get

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method retrieves information about a stage by `id`.

## Stage Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`number`](../../../data-types.md) | Identifier of the stage ||
|#

## Response Handling

HTTP status: **200**

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
                    "name": "Anthony",
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

### Returned Data

#|
|| **Name** | **Description** ||
|| **name** | Name of the stage ||
|| **code** | Symbolic code. Can be used as an external identifier ||
|| **color** | HEX color code of the stage, 6 characters ||
|| **sort** | Sorting index ||
|| **semantic** | Semantic code of the stage. Can be either `SUCCESS` or `FAIL` ||
|| **typeId** | Identifier of the process ||
|| **isFirst** | Calculated field. Returns `true` if this is the first stage of the process ||
|| **isSuccess** | Calculated field. Returns `true` if this stage is successful ||
|| **isFail** | Calculated field. Returns `true` if this stage is a failure ||
|| **tasks** | Array of tasks for the stage. Each entry has the following structure:
- `title` — title of the task
- `robotType` — type of the task. Can take one of the following values:
  - `RpaApproveActivity` — approve or reject
  - `RpaMoveActivity` — simply move
  - `RpaRequestActivity` — request information
  - `RpaReviewActivity` — review information
- `robotName` — name of the activity
- `users` — array of participants in the task for rendering in the kanban at the stage ||
|| **robotsCount** | Calculated field. Number of robots at the stage ||
|| **possibleNextStages** | Array of identifiers of stages to which the item can be moved. Not used ||
|| **permissions** | Set of permissions for the kanban:
- `droppable` — items can be moved to this stage
- `canMoveFrom` — items can be moved from this stage ||
|#

## Continue Exploring 

- [{#T}](./index.md)
- [{#T}](./rpa-stage-add.md)
- [{#T}](./rpa-stage-update.md)
- [{#T}](./rpa-stage-list-for-type.md)
- [{#T}](./rpa-stage-delete.md)