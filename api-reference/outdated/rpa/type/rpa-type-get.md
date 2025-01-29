# Get Process Information by ID rpa.type.get

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves information about a process by its `id`.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`number`](../../../data-types.md) | Identifier of the process ||
|#

## Response Handling

HTTP status: **200**

```json
{
    "type": {
        "id": 1,
        "title": "Process Name",
        "image": "list",
        "createdBy": 1,
        "settings": [],
        "permissions": [
            {
                "id": "1",
                "entity": "TYPE",
                "entityId": "1",
                "accessCode": "UA",
                "action": "ITEMS_CREATE",
                "permission": "X"
            }
        ]
    }
}
```

### Returned Data

#|
|| **Name** | **Description** ||
|| **id** | Identifier of the process ||
|| **title** | Name of the process ||
|| **image** | Icon identifier from the list ||
|| **createdBy** | Identifier of the user who created the process ||
|| **settings** | Set of process settings ||
|| **permissions** | Set of access permission settings for this process ||
|#

## Continue Exploring

- [{#T}](./index.md)
- [{#T}](./rpa-type-add.md)
- [{#T}](./rpa-type-update.md)
- [{#T}](./rpa-type-list.md)
- [{#T}](./rpa-type-delete.md)