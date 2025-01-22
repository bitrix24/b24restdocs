# Get the timeline record array for the item rpa.timeline.listForItem

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method retrieves a list of timeline records for the `itemId` of the `typeId` process.

Records are sorted in descending order by creation date, meaning the newest ones are at the top of the list.

#|
|| **Name**
`type` | **Description** ||
|| **typeId** 
[`number`](../../../data-types.md) | Identifier of the process. ||
|| **itemId** 
[`number`](../../../data-types.md) | Identifier of the item. ||
|| **start** | Offset for pagination. ||
|#

## Response Handling

HTTP status: **200**

```json
{
    "timeline": [
        {
            "id": 321,
            "typeId": 24,
            "itemId": 10,
            "createdTime": "2020-03-26T20:28:57+02:00",
            "userId": 1,
            "title": "Task Completed",
            "description": "",
            "action": "task_complete",
            "isFixed": false,
            "data": {
                "item": {
                    "name": "New name"
                },
                "scope": "task",
                "stageFrom": {
                    "id": 30,
                    "name": "Approved by Accountant"
                },
                "stageTo": {
                    "id": 31,
                    "name": "Approved"
                },
                "fields": [
                    {
                        "name": "UF_RPA_24_NAME",
                        "title": "Name"
                    }
                ],
                "task": {
                    "ID": "91",
                    "USER_ID": "1",
                    "WORKFLOW_ID": "5e7cf3e91ef413.27314358",
                    "ACTIVITY": "RpaRequestActivity",
                    "ACTIVITY_NAME": "A79985_79846_49104_50661",
                    "NAME": "Task",
                    "DESCRIPTION": "",
                    "PARAMETERS": {
                        "DOCUMENT_ID": [
                            "rpa",
                            "Bitrix\\Rpa\\Integration\\Bizproc\\Document\\Item",
                            "24:10"
                        ],
                        "TASK_EDIT_URL": "/rpa/task/id/#ID#/",
                        "ACTIONS": [
                            {
                                "color": "3bc8f5",
                                "stageId": "31",
                                "label": "Save"
                            }
                        ],
                        "FIELDS_TO_SHOW": [
                            "UF_RPA_24_NAME",
                            "UF_RPA_24_STRING_MANDATORY"
                        ],
                        "RESPONSIBLE_TYPE": null,
                        "APPROVE_TYPE": null,
                        "FIELDS_TO_SET": [
                            "UF_RPA_24_NAME"
                        ]
                    },
                    "USERS": [
                        1
                    ],
                    "INCOMPLETE_USERS": []
                }
            },
            "createdTimestamp": 1585247337000,
            "users": {
                "1": {
                    "id": "1",
                    "name": "Anton",
                    "secondName": "",
                    "lastName": "Gorbylev",
                    "title": null,
                    "workPosition": "",
                    "fullName": "Anton Gorbylev",
                    "link": "/company/personal/user/1/"
                }
            }
        }
    ]
}
```

### Returned Data

#|
|| **Name** | **Description** ||
|| **id** | Identifier of the record ||
|| **typeId** | Identifier of the process ||
|| **itemId** | Identifier of the item ||
|| **createdTime** | Time of record creation ||
|| **userId** | Identifier of the user who initiated the action ||
|| **title** | Title of the record ||
|| **description** | Text content of the record ||
|| **action** | Action type code ||
|| **isFixed** | Flag indicating if the record is attached ||
|| **data** | Serialized data about the completed action and related entities at the time of record creation. Depending on the action type, it may contain a different [set of data](#data) ||
|#

#### Data Object {#data}

#|
|| **Name** | **Description** ||
|| **item** | Data about the item:
- `item[name]` — name of the item at the time of action execution ||
|| **scope** | Action source code. Can take the value:
- `manual` — manually
- `task` — when performing a task
- `automation` — by an Automation rule
- `rest` — by an application ||
|| **stageFrom** | Data about the original stage at the time of action execution
- `id` — identifier
- `name` — name ||
|| **stageTo** | Data about the new stage, if it was changed during action execution ||
|| **fields** | Array of data about fields whose values were changed during action execution
- `name` — field code
- `title` — field title ||
|| **task** | Data about the task, if the action was performed while executing a task ||
|| **users** | Data about users who were involved in the action ||
|#

## Continue Exploring 

- [{#T}](./index.md)
- [{#T}](./rpa-timeline-add.md)
- [{#T}](./rpa-timeline-update.md)
- [{#T}](./rpa-timeline-update-is-fixed.md)
- [{#T}](./rpa-timeline-delete.md)