# Update the Attachment Flag of rpa.timeline.updateIsFixed

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [Smart scripts](../../../crm/universal/user-defined-object-types/index.md) as an alternative to this functionality.

{% endnote %}

This method updates the attachment flag of a record.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`integer`](../../../data-types.md) | Identifier of the record ||
|| **isFixed** 
[`string`](../../../data-types.md) | Attachment flag of the record. Possible values:
- `Y` — the record will be attached
- `N` — the record will not be attached ||
|#

## Response Handling

HTTP Status: **200**

```json
{
    "timeline": {
        "id": 322,
        ...
    }
}
```

## Continue Your Exploration 

- [{#T}](./index.md)
- [{#T}](./rpa-timeline-add.md)
- [{#T}](./rpa-timeline-update.md)
- [{#T}](./rpa-timeline-list-for-item.md)
- [{#T}](./rpa-timeline-delete.md)