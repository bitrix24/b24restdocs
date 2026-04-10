# Get Process Information by ID rpa.type.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Use [Smart scripts](../../../crm/universal/user-defined-object-types/index.md) as an alternative to this functionality.

{% endnote %}

This method retrieves information about a process by its `id`.

## Method Parameters

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`number`](../../../data-types.md) | Process identifier ||
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