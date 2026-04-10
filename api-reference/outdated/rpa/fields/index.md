# Field Visibility Settings: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "" %}

**DEPRECATED**

The development of methods rpa.fields.* has been halted.  
Please use the section [Smart Processes CRM](../../../crm/universal/user-defined-object-types/index.md).

{% endnote %}

> Scope: [`rpa`](../../../scopes/permissions.md)  
> Who can execute the method: any user

A set of methods for managing field visibility settings

In addition to custom fields, you can manage the visibility of system fields in the kanban card.

The system fields have the following codes:

- `id` — identifier of the entity
- `createdBy` — who created it
- `updatedBy` — who modified it
- `movedBy` — who changed the stage
- `createdTime` — creation time
- `updatedTime` — modification time
- `movedTime` — stage change time

#|  
|| **Method** | **Description** ||  
|| [rpa.fields.getSettings](./rpa-fields-get-settings.md) | Retrieves the complete set of field visibility settings for the stage with identifier `stageId` of the process with identifier `typeId` ||  
|| [rpa.fields.setSettings](./rpa-fields-set-settings.md) | Sets the complete set of field visibility settings for the stage with identifier `stageId` of the process with identifier `typeId` ||  
|| [rpa.fields.setVisibilitySettings](./rpa-fields-set-visibility-settings.md) | Changes the visibility settings `visibility` of fields `fields` for the process with identifier `typeId` at the stage with identifier `stageId` ||  
|#