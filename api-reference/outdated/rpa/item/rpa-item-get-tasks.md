# Get Data on Current Tasks of the rpa.item.getTasks Element

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Required parameters are not specified
- Examples are missing
- Response in case of an error is absent

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.item.getTasks` will return data on the current tasks of the element with the identifier id of the process with the identifier typeId.

#|
|| **Parameter** / **Type** | **Description** ||
|| **typeId** 
[`number`](../../../data-types.md) | Identifier of the process. ||
|| **id** 
[`number`](../../../data-types.md) | Identifier of the element. ||
|#

## Response in Case of Success

> 200 OK

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