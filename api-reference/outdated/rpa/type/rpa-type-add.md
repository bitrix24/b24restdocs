# Create Process rpa.type.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Use [Smart scripts](../../../crm/universal/user-defined-object-types/index.md) as an alternative to this functionality.

{% endnote %}

This method creates a new process.

## Method Parameters

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`array`](../../../data-types.md) | A list of process fields. The list of possible fields is described [below](#fields) ||
|#

### Parameter fields {#fields}

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **title***
[`string`](../../../data-types.md) | The name of the process ||
|| **image**
[`string`](../../../data-types.md) | The image of the process from the list ||
|| **settings**
[`array`](../../../data-types.md) | A list of arbitrary settings for the process ||
|| **permissions**
[`array`](../../../data-types.md) | A list of objects. Each object describes access permissions for this process ||
|#

{% note warning %}

- Automated scenarios, such as creating stages, Automation rules, and default fields, will not be triggered when creating a process via `rest`.
- The request must specify access permissions for modifying the process.

{% endnote %}

## Code Examples

Create a new process named "My Process." All users can create items for this process. Only the user with `id = 1` can modify the settings of this process.

{% include [Example Note](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    {
        "fields": {
            "title": "My Process",
            "image": "list",
            "permissions": [
                {
                    "accessCode": "UA",
                    "permission": "X",
                    "action": "ITEMS_CREATE"
                },
                {
                    "accessCode": "U1",
                    "permission": "X",
                    "action": "MODIFY"
                },
            ]
        }
    }
    ```

{% endlist %}

## Response Handling

The method will return data in the response similar to the response of the method [rpa.type.get](./rpa-type-get.md).

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./rpa-type-update.md)
- [{#T}](./rpa-type-get.md)
- [{#T}](./rpa-type-list.md)
- [{#T}](./rpa-type-delete.md)