# Get an array of timeline records for the item rpa.timeline.listForItem

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.timeline.listForItem` returns an array of timeline records for the item with the identifier itemId of the process with the identifier itemId, sorted in descending order by creation date (newest at the top).

#|
|| **Parameter** / **Type** | **Description** ||
|| **typeId** 
[`number`](../../../data-types.md) | Process identifier. ||
|| **itemId** 
[`number`](../../../data-types.md) | Item identifier. ||
|| **start** | Offset for pagination. ||
|#

## Response on success

> 200 OK

```json
{
    "timeline": [
        {
            "id": 321,
            "typeId": 24,
            "itemId": 10,
            "createdTime": "2020-03-26T20:28:57+02:00",
            "userId": 1,
            "title": "Task completed",
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
                    "name": "Approved by accountant"
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

- `id` - record identifier
- `typeId` - process identifier
- `itemId` - item identifier
- `createdTime` - record creation time
- `userId` - identifier of the user who initiated the action
- `title` - record title
- `description` - text content of the record
- `action` - action type code
- `isFixed` - record attachment flag
- `data` - serialized data about the completed action and related entities at the time of record creation. Depending on the action type, it may contain a different set of data. The main parameters are:
- `item` - data about the item
    - `item[name]` - name of the item at the time of action execution
- `scope` - action source code, can take one of the following values:
    - `manual` - manually
    - `task` - when performing a task
    - `automation` - by Automation rule
    - `rest` - by application
- `stageFrom` - data about the original stage at the time of action execution
    - `id` - identifier
    - `name` - name
- `stageTo` - data about the new stage (if it was changed during action execution)
- `fields` - array of data about fields whose values were changed during action execution
    - `name` - field code
    - `title` - field title
- `task` - data about the task (if the action was performed while executing a task)
- `users` - data about users who were involved in the action