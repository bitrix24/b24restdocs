# Get Data on Current Tasks of the Element rpa.item.getTasks

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Use [Smart processes](../../../crm/universal/user-defined-object-types/index.md) as an alternative to this functionality.

{% endnote %}

This method retrieves data on the current tasks of the element with the identifier `id` for the process with the identifier `typeId`.

## Method Parameters

#| 
|| **Name**
`type` | **Description** ||
|| **typeId** 
[`integer`](../../../data-types.md) | Identifier of the process ||
|| **id** 
[`integer`](../../../data-types.md) | Identifier of the element ||
|#

## Response Handling

HTTP Status: **200**

```json
{
    "tasks": [
        {
            "id": "93",
            "title": "asdf",
            "description": "",
            "userId": 1,
            "data": {
                "participantJoint": "or",
                "isMine": true,
                "controls": {
                    "BUTTONS": [
                        {
                            "TYPE": "submit",
                            "TARGET_USER_STATUS": 3,
                            "NAME": "complete",
                            "VALUE": "Y",
                            "TEXT": "Save",
                            "COLOR": "3bc8f5"
                        }
                    ]
                },
                "type": "RpaRequestActivity",
                "url": "/rpa/task/id/93/",
                "fieldsToShow": null,
                "fieldsToSet": [
                    "Title"
                ],
                "users": [
                    {
                        "id": 1,
                        "status": 0
                    }
                ]
            },
            "itemClassName": "BX.Rpa.Timeline.Task",
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

## Continue Your Exploration 

- [{#T}](./index.md)
- [{#T}](./rpa-item-add.md)
- [{#T}](./rpa-item-update.md)
- [{#T}](./rpa-item-get.md)
- [{#T}](./rpa-item-list.md)
- [{#T}](./rpa-item-delete.md)