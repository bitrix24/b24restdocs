# Get Information About Stage rpa.stage.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Use [Smart scripts](../../../crm/universal/user-defined-object-types/index.md) as an alternative to this functionality.

{% endnote %}

This method retrieves information about a stage by its `id`.

## Stage Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../../data-types.md) | Identifier of the stage ||
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
                    "name": "Anton",
                    "photoSrc": "",
                    "url": "\/company\/personal\/user\/1\/",
                    "entityType": "users"
                },
                {
                    "id": "U4",
                    "entityId": 4,
                    "name": "Piter",
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
|| **isFirst** | Computed field. Returns `true` if this is the first stage of the process ||
|| **isSuccess** | Computed field. Returns `true` if this stage is successful ||
|| **isFail** | Computed field. Returns `true` if this stage has failed ||
|| **tasks** | Array of tasks for the stage. Each entry has the following structure:
- `title` — title of the task
- `robotType` — type of the task. Can take one of the following values:
  - `RpaApproveActivity` — approve or reject
  - `RpaMoveActivity` — simply move
  - `RpaRequestActivity` — request information
  - `RpaReviewActivity` — review information
- `robotName` — name of the activity
- `users` — array of participants for the task to be displayed in the kanban at the stage ||
|| **robotsCount** | computed field. Number of robots at the stage ||
|| **possibleNextStages** | array of identifiers of stages to which the item can be moved. Not used ||
|| **permissions** | set of permissions for the kanban:
- `droppable` — items can be moved to this stage
- `canMoveFrom` — items can be moved from this stage ||
|#

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./rpa-stage-add.md)
- [{#T}](./rpa-stage-update.md)
- [{#T}](./rpa-stage-list-for-type.md)
- [{#T}](./rpa-stage-delete.md)