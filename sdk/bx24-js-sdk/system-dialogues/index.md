# System Dialogs: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

System dialogs open within the Bitrix24 interface and operate outside the application frame. They are used when there is a need to select a user, access permissions, or CRM entities through the platform's standard windows.

> Quick Navigation: [All Methods](#all-methods)

## How to Select a Dialog

1. To select employees, use [BX24.selectUser](./bx24-select-user.md) for a single user and [BX24.selectUsers](./bx24-select-users.md) for multiple users.
2. To select access permissions, use [BX24.selectAccess](./bx24-select-access.md).
3. To select CRM entities, use [BX24.selectCRM](./bx24-select-crm.md).

## What a Workflow Scenario Looks Like

1. Determine what data is needed for the next step: user ID, access codes, or CRM entities.
2. Pass a `callback` handler to receive the selected values after the dialog is closed.
3. Normalize the data from the `callback` into the required format: a single `id`, an array of `id`s, or access codes.
4. Use the obtained values in the next application action: REST call, filter, permission settings, or form filling.
5. Save the selection result in the interface state so that the user can continue the scenario without re-selection.

## Relationships with Other Objects

**User.** The methods [BX24.selectUser](./bx24-select-user.md) and [BX24.selectUsers](./bx24-select-users.md) return the selected employees to the `callback` handler and assist in passing their identifiers to subsequent REST calls.

**Access Permissions.** The method [BX24.selectAccess](./bx24-select-access.md) returns access permission codes in the format `U1`, `SG4`, `AU` and helps configure visibility rules in objects where access codes are needed.

**CRM.** The method [BX24.selectCRM](./bx24-select-crm.md) returns selected CRM entities by types (`lead`, `contact`, `company`, `deal`, `quote`) and helps utilize them in integration scenarios.

## Overview of Methods {#all-methods}

#|
|| **Method** | **Description** ||
|| [BX24.selectUser](./bx24-select-user.md) | Displays the standard dialog for selecting a single user ||
|| [BX24.selectUsers](./bx24-select-users.md) | Displays the standard dialog for selecting multiple users ||
|| [BX24.selectAccess](./bx24-select-access.md) | Displays the standard dialog for selecting access permissions ||
|| [BX24.selectCRM](./bx24-select-crm.md) | Displays the standard dialog for selecting CRM entities ||
|#