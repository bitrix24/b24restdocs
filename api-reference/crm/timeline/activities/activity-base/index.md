# Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- No content available
- Replace the link

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

Methods for working with system activities in the [timeline](../../index.md)

#|
|| **Method** | **Description** ||
|| [crm.activity.add](./crm-activity-add.md) | Adds a new activity to the timeline ||
|| [crm.activity.update](./crm-activity-update.md) | Updates an activity ||
|| [crm.activity.get](./crm-activity-get.md) | Retrieves information about an activity by its ID ||
|| [crm.activity.list](./crm-activity-list.md) | Retrieves a list of all activities for a CRM entity by filter ||
|| [crm.activity.delete](./crm-activity-delete.md) | Deletes any type of activities ||
|| [crm.activity.fields](./crm-activity-fields.md) | Retrieves the description of fields for an activity ||
|| [crm.activity.communication.fields](./crm-activity-communication-fields.md) | Retrieves the description for communication fields in an activity ||
|#

## Additional Information

- [CRM Entity Type](../../../data-types.md#object_type) 