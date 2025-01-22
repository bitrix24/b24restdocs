# Get Information About the rpa.item.get Element

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Required parameters are not specified
- Examples are missing
- Response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.item.get` returns information about the element with the identifier id of the process with the identifier typeId.

#|
|| **Name**
`type` | **Description** ||
|| **typeId** 
[`number`](../../../data-types.md) | Identifier of the process. ||
|| **id** 
[`number`](../../../data-types.md) | Identifier of the element. ||
|#

## Response in Case of Success

> 200 OK

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
        "createdTime": "03.19.2020 13:07:39",
        "updatedTime": "03.23.2020 18:34:05",
        "movedTime": "03.23.2020 18:34:05",
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

Here:

- `stageId` - identifier of the stage the element is currently in
- `previousStageId` - identifier of the previous stage of the element
- `name` - name of the element
- `typeId` - identifier of the process
- `createdBy` - id of the user who created the element
- `updatedBy` - id of the user who modified the element
- `movedBy` - id of the user who changed the stage of the element
- `createdTime` - time the element was created
- `updatedTime` - time the element was modified
- `movedTime` - time the stage of the element was changed
- `detailUrl` - link to the detail form of the element
- `tasksCounter` - number of tasks on the element for the user
- `tasksFaces` - information for rendering the sequence of responsible parties during approval
- `completed` - who completed the task
- `running` - who is currently working on the task
- `all` - all participants
- `users` - aggregated information about all users related to the element. A list where the key is the user id
- `id` - identifier
- `name` - first name
- `secondName` - middle name
- `lastName` - last name
- `title` - title
- `workPosition` - job position
- `fullName` - formatted name
- `link` - link to the profile
- `UF_RPA_...` - values of custom fields
- values of multiple fields are returned as an array
- value of a "file" type field is returned as a list
    - `id` - identifier
    - `url` - link to the file on the account
    - `urlMachine` - link to the file for the application