# Mail recipients: method overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Mail recipients in Bitrix24 are contacts and employees that can be selected when addressing an e-mail.

The methods in this section allow you to:
- search for contacts from the address book using a search string
- search for employees by name and e-mail
- retrieve a list of recipient fields and the description of a specific field

> Quick links: [all methods](#all-methods)
>
> User documentation: [Manage emails in Bitrix24](https://helpdesk.bitrix24.com/open/20134658/)

## When to use each method

Use [mail.recipient.listcontacts](./mail-recipient-listcontacts.md) when you need to select external recipients from the address book for an e-mail.

Use [mail.recipient.listemployees](./mail-recipient-listemployees.md) when you need to find a Bitrix24 employee by name or e-mail.

Use [mail.recipient.field.list](./mail-recipient-field-list.md) and [mail.recipient.field.get](./mail-recipient-field-get.md) when you need to:
- find available recipient fields
- retrieve the types and metadata of a specific field

## Workflow for working with recipients

1. Select the e-mail recipients: external recipients via [mail.recipient.listcontacts](./mail-recipient-listcontacts.md), employees via [mail.recipient.listemployees](./mail-recipient-listemployees.md)
2. Verify the recipient data structure: the list of fields via [mail.recipient.field.list](./mail-recipient-field-list.md) and the field description via [mail.recipient.field.get](./mail-recipient-field-get.md)

## Section limitations

- Search results depend on the search string and data accessibility for the current user
- The methods in this section select recipients and do not perform e-mail sending

## Connection with Other Objects

**E-mail.** The methods in this section help select recipients for sending and replying to e-mails in the [mail.message.*](../message/index.md) methods.

## Overview of Methods {#all-methods}

> Scope: [`mail`](../../../scopes/permissions.md)
>
> Who can execute the methods: any user

#|
|| **Method** | **Description** ||
|| [mail.recipient.listcontacts](./mail-recipient-listcontacts.md) | Searches for contacts from the address book ||
|| [mail.recipient.listemployees](./mail-recipient-listemployees.md) | Searches for employees by name or e-mail ||
|| [mail.recipient.field.list](./mail-recipient-field-list.md) | Returns a list of recipient fields ||
|| [mail.recipient.field.get](./mail-recipient-field-get.md) | Returns the description of a recipient field ||
|#

## Continue Learning

- [{#T}](../message/index.md)
- [{#T}](../mailbox/index.md)
- [{#T}](../index.md)