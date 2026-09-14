# Get Process Information by ID rpa.type.get

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

This method retrieves information about a process by its `id`.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../../data-types.md) | Process identifier ||
|#

## Response Handling

HTTP Status: **200**

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
|| **id** | Process identifier ||
|| **title** | Process name ||
|| **image** | Icon identifier from the list ||
|| **createdBy** | Identifier of the user who created the process ||
|| **settings** | Set of process settings ||
|| **permissions** | Set of access permissions for this process ||
|#

## Continue Exploring

- [{#T}](./index.md)
- [{#T}](./rpa-type-add.md)
- [{#T}](./rpa-type-update.md)
- [{#T}](./rpa-type-list.md)
- [{#T}](./rpa-type-delete.md)