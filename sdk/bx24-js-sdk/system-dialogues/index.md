# System Dialogs: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

System dialogs are the standard selection windows of Bitrix24. The application invokes a dialog from its own frame, while Bitrix24 renders the window on top of that frame, so the application does not have to assemble a list of users or CRM entities and track access permissions. Dialogs are used when there is a need to select a user, access permissions, or CRM entities.

> Quick Navigation: [All Methods](#all-methods)

## How to Select a Dialog

1. If you need to select a single employee, use [BX24.selectUser](./bx24-select-user.md), and [BX24.selectUsers](./bx24-select-users.md) for multiple ones.
2. If you need to select access permissions, use [BX24.selectAccess](./bx24-select-access.md).
3. If you need to select leads, contacts, companies, deals, or quotes, use [BX24.selectCRM](./bx24-select-crm.md).

## What the Handler Receives

Dialogs do not return data directly. The selection result arrives in the `callback` function that you pass when calling the method.

#|
|| **Method** | **What the `callback` Receives** ||
|| [BX24.selectUser](./bx24-select-user.md) | An object `{id, name}` of the selected user ||
|| [BX24.selectUsers](./bx24-select-users.md) | An array of `{id, name}` objects ||
|| [BX24.selectAccess](./bx24-select-access.md) | An array of `{id, name}` objects, where `id` is an access code such as `U1`, `SG4`, `AU` ||
|| [BX24.selectCRM](./bx24-select-crm.md) | An object with the keys `lead`, `contact`, `company`, `deal`, `quote`. Each key holds an array of the selected entities ||
|#

The handler is triggered only when the selection is confirmed. If the user closes the dialog without selecting anything, the handler is not called. Dialogs return no error codes.

## Key Considerations

- A dialog can be invoked only from an application embedded in Bitrix24, and only after [BX24.init](../system-functions/bx24-init.md)
- Dialogs require no scope of their own: they open the Bitrix24 interface instead of calling the REST API. Permissions and scope are checked in the methods you call with the identifiers you receive
- In a dialog, the user sees only the objects they have access to
- The `id` of a CRM entity arrives as a composite value with a type prefix, for example `L_1348` for a lead and `C_2` for a contact. Use the numeric part as the object identifier in CRM methods

## Relationships with Other Objects

**User.** The methods [BX24.selectUser](./bx24-select-user.md) and [BX24.selectUsers](./bx24-select-users.md) return the numeric `id` of an employee. This identifier is passed to Bitrix24 methods that expect a `USER_ID`: for example, to [user.get](../../../api-reference/user/user-get.md) to retrieve employee details, or to the person responsible field when creating objects.

**Access Permissions.** The method [BX24.selectAccess](./bx24-select-access.md) returns access codes — `U1` for a user, `SG4` for a workgroup, `AU` for all authorized users. Such codes are accepted by the visibility and permission parameters of other Bitrix24 objects, where the list of recipients is defined by a set of codes rather than by a single user.

**CRM.** The method [BX24.selectCRM](./bx24-select-crm.md) returns the selected leads, contacts, companies, deals, and quotes. The numeric part of `id` is passed to the methods of the corresponding CRM entity, for example to [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md), and the entity type is taken from the response key.

**Application.** A dialog is invoked after the library is initialized — this is described in the [Initialization and Authorization](../system-functions/index.md) section. The selected identifiers are passed to Bitrix24 methods with the help of the [Calling REST Methods](../how-to-call-rest-methods/index.md) section, and so that the user does not select the same values at every launch, the result is retained with the functions of the [App Configurations](../options/index.md) section.

## Overview of Methods {#all-methods}

#|
|| **Method** | **Description** ||
|| [BX24.selectUser](./bx24-select-user.md) | Displays the standard dialog for selecting a single user ||
|| [BX24.selectUsers](./bx24-select-users.md) | Displays the standard dialog for selecting multiple users ||
|| [BX24.selectAccess](./bx24-select-access.md) | Displays the standard dialog for selecting access permissions ||
|| [BX24.selectCRM](./bx24-select-crm.md) | Displays the standard dialog for selecting CRM entities ||
|#