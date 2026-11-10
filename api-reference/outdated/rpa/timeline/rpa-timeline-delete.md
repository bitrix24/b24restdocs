# Delete Timeline Record rpa.timeline.delete

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Use [Smart scripts](../../../crm/universal/user-defined-object-types/index.md) as an alternative functionality.

{% endnote %}

This method deletes a timeline record with the identifier `id`.

The method only deletes records that were created by the same user and through the application.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the record ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "RPA_ITEM_VIEW_ACCESS_DENIED",
    "error_description": "You don't have permission to view this item."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `RPA_ITEM_VIEW_ACCESS_DENIED` | You don't have permission to view this item | The user cannot view the element the entry belongs to. All Bitrix24 employees can view elements; they are not available to extranet users ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Your Exploration 

- [{#T}](./index.md)
- [{#T}](./rpa-timeline-add.md)
- [{#T}](./rpa-timeline-update.md)
- [{#T}](./rpa-timeline-update-is-fixed.md)
- [{#T}](./rpa-timeline-list-for-item.md)