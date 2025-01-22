# Create a New Timeline Entry rpa.timeline.add

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method will create a new timeline entry for the `itemId` of the `typeId` process.

This method allows modifying only the `title` and `description` fields.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **typeId** 
[`number`](../../../data-types.md) | Process identifier ||
|| **itemId** 
[`number`](../../../data-types.md) | Entity identifier ||
|| **fields** 
[`array`](../../../data-types.md) | Entry fields ||
|#

### Fields Parameter

#|
|| **Name**
`type` | **Description** ||
|| **title** 
[`string`](../../../data-types.md) | Entry title ||
|| **description** 
[`string`](../../../data-types.md) | Entry description. HTML tags can be used ||
|#

## Response Handling

HTTP Status: **200**

```json
{
    "timeline": {
        "id": 325,
        "typeId": 24,
        "itemId": 10,
        "createdTime": "2020-03-26T21:55:25+02:00",
        "userId": 1,
        "title": "rest update",
        "description": "<h5>small header</h5>",
        "action": false,
        "isFixed": false,
        "data": {
            "scope": "rest"
        },
        "createdTimestamp": 1585252525000,
        "users": {
            "1": {
                "id": "1",
                "name": "Anthony",
                "secondName": "",
                "lastName": "",
                "title": null,
                "workPosition": "",
                "fullName": "Anthony",
                "link": "/company/personal/user/1/"
            }
        }
    }
}
```

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./rpa-timeline-update.md)
- [{#T}](./rpa-timeline-update-is-fixed.md)
- [{#T}](./rpa-timeline-list-for-item.md)
- [{#T}](./rpa-timeline-delete.md)