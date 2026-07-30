# Mail Services: Methods Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

In Bitrix24, you can connect mailboxes from services such as Gmail, Outlook, and others. This simplifies working with emails.

> Quick navigation: [All Methods](#all-methods)

The methods in this section manage mail services. Mailboxes, emails, and recipients are managed via the [Webmail REST 3.0](../mail/index.md) methods.

## Getting Started

1. Create a mail service using the [mailservice.add](./mailservice-add.md) method
2. Update service parameters using the [mailservice.update](./mailservice-update.md) method
3. Retrieve a service by identifier using the [mailservice.get](./mailservice-get.md) method
4. Check the list of active services using the [mailservice.list](./mailservice-list.md) method
5. Delete a service using the [mailservice.delete](./mailservice-delete.md) method if it is no longer needed
6. To check available fields, use the [mailservice.fields](./mailservice-fields.md) method

## Mail Service Identifiers

- `ID` — the mail service identifier. It is returned by the [mailservice.add](./mailservice-add.md) method
- Retrieve service parameters by `ID` using the [mailservice.get](./mailservice-get.md) method
- Find active mail services and their `ID` using the [mailservice.list](./mailservice-list.md) method

{% note tip "User documentation" %}

- [How to Connect Gmail to Bitrix24](https://helpdesk.bitrix24.com/open/18508706/)
- [How to Connect Gmail to Bitrix24](https://helpdesk.bitrix24.com/open/19264454/)
- [Ways to Connect Mailboxes in Bitrix24](https://helpdesk.bitrix24.com/open/20134658/)
- [How to Work with Webmail in Bitrix24](https://helpdesk.bitrix24.com/open/8293717/)
- [Questions About Webmail (Connection, CRM Integration)](https://helpdesk.bitrix24.com/open/24207776/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`mailservice`](../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [mailservice.add](./mailservice-add.md) | Creates an email service ||
|| [mailservice.update](./mailservice-update.md) | Updates the parameters of the email service ||
|| [mailservice.get](./mailservice-get.md) | Returns the parameters of the email service by ID ||
|| [mailservice.list](./mailservice-list.md) | Returns a list of active email services for the current site ||
|| [mailservice.delete](./mailservice-delete.md) | Deletes an email service ||
|| [mailservice.fields](./mailservice-fields.md) | Returns the names of the fields of the email service ||
|#
