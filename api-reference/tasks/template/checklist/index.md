# Task Template Checklist Methods: Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods in this section manage checklist items within task templates.

For more information about checklists, refer to the article [Checklists: Overview of Methods](../../checklist-item/index.md).

> Quick Navigation: [All Methods](#all-methods)

## Overview of Methods {#all-methods}

> Scope: [task](../../../scopes/permissions.md)
>
> Who can execute the method: varies by method

#|
|| **Method** | **Description** ||
|| [tasks.template.checklist.add](./tasks-template-checklist-add.md) | Adds a checklist item ||
|| [tasks.template.checklist.update](./tasks-template-checklist-update.md) | Updates a checklist item ||
|| [tasks.template.checklist.get](./tasks-template-checklist-get.md) | Retrieves a checklist item by id ||
|| [tasks.template.checklist.list](./tasks-template-checklist-list.md) | Retrieves a list of checklist items ||
|| [tasks.template.checklist.delete](./tasks-template-checklist-delete.md) | Deletes a checklist item ||
|| [tasks.template.checklist.moveAfter](./tasks-template-checklist-move-after.md) | Moves an item after the specified one ||
|| [tasks.template.checklist.moveBefore](./tasks-template-checklist-move-before.md) | Moves an item before the specified one ||
|| [tasks.template.checklist.complete](./tasks-template-checklist-complete.md) | Marks an item as completed ||
|| [tasks.template.checklist.renew](./tasks-template-checklist-renew.md) | Returns an item to an incomplete state ||
|| [tasks.template.checklist.addAttachmentByContent](./tasks-template-checklist-add-attachment-by-content.md) | Adds an attachment from content ||
|| [tasks.template.checklist.addAttachmentsFromDisk](./tasks-template-checklist-add-attachments-from-disk.md) | Adds attachments from Drive ||
|| [tasks.template.checklist.removeAttachments](./tasks-template-checklist-remove-attachments.md) | Removes attachments from a checklist item ||
|#