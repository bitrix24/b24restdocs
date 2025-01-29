# Get Information About the rpa.item.get Element

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves information about the element with the identifier `id` of the process with the identifier `typeId`.

#|
|| **Name**
`type` | **Description** ||
|| **typeId** 
[`integer`](../../../data-types.md) | Process identifier ||
|| **id** 
[`integer`](../../../data-types.md) | Element identifier ||
|#

## Response Handling

HTTP Status: **200**

```json
{
    "item": {
        "id": 43,
        "stageId": 4,
        "previousStageId": 3,
        "name": "Element Name",
        "typeId": 1,
        "createdBy": 1,
        "updatedBy": 1,
        "createdTime": "03/19/2020 13:07:39",
        "updatedTime": "03/23/2020 18:34:05",
        "movedTime": "03/23/2020 18:34:05",
        "detailUrl": "/rpa/item/1/43/",
        "movedBy": 1,
        "UF_RPA_1_NAME": "Element Name",
        "tasksCounter": 0,
        "tasksFaces": {
            "completed": [
                1
            ],
            "running": [],
            "all": [
                1
            ]
        },
        "users": {
            "1": {
                "id": "1",
                "name": "Anthony",
                "secondName": null,
                "lastName": "",
                "title": null,
                "workPosition": null,
                "fullName": "Anthony",
                "link": "/company/personal/user/1/"
            }
        }
    }
}
```

### Returned Data

#|
|| **Name** | **Description** ||
|| **stageId** | Identifier of the stage the element is currently in ||
|| **previousStageId** | Identifier of the previous stage of the element ||
|| **name** | Name of the element ||
|| **typeId** | Identifier of the process ||
|| **createdBy** | Identifier of the user who created the element ||
|| **updatedBy** | Identifier of the user who modified the element ||
|| **movedBy** | Identifier of the user who changed the stage of the element ||
|| **createdTime** | Time the element was created ||
|| **updatedTime** | Time the element was modified ||
|| **movedTime** | Time the stage of the element was changed ||
|| **detailUrl** | Link to the element's detail form ||
|| **tasksCounter** | Number of tasks associated with the element for the user ||
|| **tasksFaces** | Information for rendering the sequence of responsible parties during approval:
- `completed` — who completed the task
- `running` — who is currently working on the task
- `all` — all participants ||
|| **users** | Aggregated information about all users related to the element. A list where the key is the user identifier:
- `id` — identifier
- `name` — first name
- `secondName` — middle name
- `lastName` — last name
- `title` — title
- `workPosition` — job title
- `fullName` — formatted name
- `link` — link to the profile ||
|| **UF_RPA_...** | Values of custom fields.

Values of multiple fields are returned as an array.

File type field values are returned as a list:
- `id` — identifier
- `url` — link to the file on the account
- `urlMachine` — link to the file for the application ||
|#

## Continue Exploring 

- [{#T}](./index.md)
- [{#T}](./rpa-item-add.md)
- [{#T}](./rpa-item-update.md)
- [{#T}](./rpa-item-get-tasks.md)
- [{#T}](./rpa-item-list.md)
- [{#T}](./rpa-item-delete.md)