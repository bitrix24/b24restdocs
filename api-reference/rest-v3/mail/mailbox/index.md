# Mailboxes: methods overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Mailboxes in Bitrix24 store the user's email connection parameters and a set of available sender addresses.

Using the methods in this section, you can:

- retrieve a list of connected mailboxes
- request mailbox data by its identifier
- find available sender addresses
- view a list of mailbox fields and the description of a specific field

> Quick links: [all methods](#all-methods)
>
> User documentation: [Connect mailboxes to Bitrix24](https://helpdesk.bitrix24.com/open/19264454/)

## When to use each method

Use [mail.mailbox.list](./mail-mailbox-list.md) when you need to retrieve a list of available mailboxes and select a `id` for further requests.

Use [mail.mailbox.get](./mail-mailbox-get.md) when you need to retrieve the parameters of a specific mailbox by its `id`.

Use [mail.mailbox.senders](./mail-mailbox-senders.md) when you need to retrieve a list of sender addresses available to the current user.

Use [mail.mailbox.field.list](./mail-mailbox-field-list.md) and [mail.mailbox.field.get](./mail-mailbox-field-get.md) when you need to:
- find available mailbox fields
- retrieve the types and metadata of a specific field

## Workflow for working with mailboxes

1. Retrieve a list of mailboxes via [mail.mailbox.list](./mail-mailbox-list.md)
2. Select the required `id` and retrieve details via [mail.mailbox.get](./mail-mailbox-get.md)
3. Retrieve sender addresses via [mail.mailbox.senders](./mail-mailbox-senders.md) before sending an email
4. Clarify the field structure via `*.field.list` and `*.field.get`

## Section limitations

- The set of available mailboxes and senders depends on the current user's permissions
- The methods in this section do not send emails, but prepare data for working with emails

{% note tip "User documentation" %}

- [Connect Gmail mailbox to Bitrix24](https://helpdesk.bitrix24.com/open/25811739/)
- [Connect several email accounts in Bitrix24](https://helpdesk.bitrix24.com/open/25842131/)
- [Disable a mailbox](https://helpdesk.bitrix24.com/open/21970368/)

{% endnote %}

## Connection with Other Objects

**Emails.** The mailbox identifier is used in [mail.message.*](../message/index.md) methods to search for and process emails in the required mailbox.

## Overview of Methods {#all-methods}

> Scope: [`mail`](../../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

#|
|| **Method** | **Description** ||
|| [mail.mailbox.list](./mail-mailbox-list.md) | Returns a list of the current user's mailboxes ||
|| [mail.mailbox.get](./mail-mailbox-get.md) | Returns a mailbox by identifier ||
|| [mail.mailbox.senders](./mail-mailbox-senders.md) | Returns sender addresses available to the current user ||
|| [mail.mailbox.field.list](./mail-mailbox-field-list.md) | Returns a list of mailbox fields ||
|| [mail.mailbox.field.get](./mail-mailbox-field-get.md) | Returns a description of a mailbox field ||
|#

## Continue Learning

- [{#T}](../message/index.md)
- [{#T}](../recipient/index.md)
- [{#T}](../index.md)
