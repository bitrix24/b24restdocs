# Webmail in REST 3.0: section overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Webmail in Bitrix24 allows you to work with e-mails directly in the interface: read and send e-mails, filter correspondence, link e-mails to the CRM, and also create a chat, a task, a calendar event, or a Feed message from an e-mail.

Mail methods work with three groups of objects:

- [Mailboxes](./mailbox/index.md) — show a list of user mailboxes, mailbox data, and available sender addresses
- [E-mails](./message/index.md) — find and retrieve e-mails, send and process an e-mail, and create linked objects from an e-mail
- [Recipients](./recipient/index.md) — find contacts and employees for addressing e-mails and show recipient fields

> Quick links: [all methods](#all-methods)
>
> User documentation: [Manage emails in Bitrix24](https://helpdesk.bitrix24.com/open/20134658/)

## Getting Started

1. Retrieve a list of mailboxes using the [mail.mailbox.list](./mailbox/mail-mailbox-list.md) method
2. Select a mailbox and retrieve the required e-mails using the [mail.message.list](./message/mail-message-list.md) method
3. Retrieve an e-mail by identifier using the [mail.message.get](./message/mail-message-get.md) method
4. Perform an action with an e-mail: send a reply, move an e-mail, or create a linked object

To manage mail services, use the methods in the [mailservice.*](../../mailservice/index.md) section.

## Limitations and recommendations

- The [mail.mailbox.*](./mailbox/index.md) and [mail.message.*](./message/index.md) methods only work with mailboxes to which the current user has access
- Use the `*.field.list` and `*.field.get` methods to check available fields and their types

{% note tip "User documentation" %}

- [Configure access permissions to Bitrix24 Mail](https://helpdesk.bitrix24.com/open/25842003/)

{% endnote %}

## Connection with Other Objects

**Mailbox.** The [mail.mailbox.*](./mailbox/index.md) methods work with the mailboxes of the current user. The mailbox identifier `id` is used in methods for retrieving a mailbox and searching for e-mails. Available mailboxes can be retrieved using the [mail.mailbox.list](./mailbox/mail-mailbox-list.md) method.

**E-mail.** The [mail.message.*](./message/index.md) methods work with e-mails available to the current user. The e-mail identifier `id` is required to retrieve an e-mail, reply to it, forward it, move it, or create a linked object.

**Recipient.** The [mail.recipient.*](./recipient/index.md) methods help select e-mail recipients. Contacts can be found using the [mail.recipient.listcontacts](./recipient/mail-recipient-listcontacts.md) method, and employees via the [mail.recipient.listemployees](./recipient/mail-recipient-listemployees.md) method.

**CRM.** An e-mail can be linked to a CRM activity using the [mail.message.createcrmactivity](./message/mail-message-createcrmactivity.md) method. The link can be removed using the [mail.message.removecrmactivity](./message/mail-message-removecrmactivity.md) method.

**Task.** An e-mail can be converted into a task using the [mail.message.createtask](./message/mail-message-createtask.md) method

**Calendar.** A calendar event can be created from an e-mail using the [mail.message.createcalendarevent](./message/mail-message-createcalendarevent.md) method

**Chat.** The text of an e-mail can be discussed in a chat using the [mail.message.createchat](./message/mail-message-createchat.md) method

**News feed.** An e-mail can be published as a post in the News feed using the [mail.message.createfeedpost](./message/mail-message-createfeedpost.md) method

## Overview of Methods {#all-methods}

> Scope: [`mail`](../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

### Mailboxes

#|
|| **Method** | **Description** ||
|| [mail.mailbox.list](./mailbox/mail-mailbox-list.md) | Returns a list of the current user's mailboxes ||
|| [mail.mailbox.get](./mailbox/mail-mailbox-get.md) | Returns a mailbox by identifier ||
|| [mail.mailbox.senders](./mailbox/mail-mailbox-senders.md) | Returns sender addresses available to the current user ||
|| [mail.mailbox.field.list](./mailbox/mail-mailbox-field-list.md) | Returns a list of mailbox fields ||
|| [mail.mailbox.field.get](./mailbox/mail-mailbox-field-get.md) | Returns a mailbox field description ||
|#

### E-mails

#|
|| **Method** | **Description** ||
|| [mail.message.list](./message/mail-message-list.md) | Searches for e-mails in the current user's mailboxes ||
|| [mail.message.get](./message/mail-message-get.md) | Returns an e-mail by identifier ||
|| [mail.message.thread](./message/mail-message-thread.md) | Returns an e-mail thread by the identifier of a single e-mail ||
|| [mail.message.send](./message/mail-message-send.md) | Sends a new e-mail ||
|| [mail.message.reply](./message/mail-message-reply.md) | Sends a reply to an e-mail ||
|| [mail.message.forward](./message/mail-message-forward.md) | Forwards an e-mail ||
|| [mail.message.movetofolder](./message/mail-message-movetofolder.md) | Moves e-mails to a folder, spam, or trash ||
|| [mail.message.createcrmactivity](./message/mail-message-createcrmactivity.md) | Creates a CRM activity from an e-mail ||
|| [mail.message.removecrmactivity](./message/mail-message-removecrmactivity.md) | Removes the link between an e-mail and a CRM activity ||
|| [mail.message.createtask](./message/mail-message-createtask.md) | Creates a task from an e-mail ||
|| [mail.message.createcalendarevent](./message/mail-message-createcalendarevent.md) | Creates a calendar event from an e-mail ||
|| [mail.message.createchat](./message/mail-message-createchat.md) | Creates a chat from an e-mail ||
|| [mail.message.createfeedpost](./message/mail-message-createfeedpost.md) | Creates a News Feed message from an e-mail ||
|| [mail.message.field.list](./message/mail-message-field-list.md) | Returns a list of e-mail fields ||
|| [mail.message.field.get](./message/mail-message-field-get.md) | Returns an e-mail field description ||
|#

### Recipients

#|
|| **Method** | **Description** ||
|| [mail.recipient.listcontacts](./recipient/mail-recipient-listcontacts.md) | Searches for contacts in the address book ||
|| [mail.recipient.listemployees](./recipient/mail-recipient-listemployees.md) | Searches for employees by name or e-mail ||
|| [mail.recipient.field.list](./recipient/mail-recipient-field-list.md) | Returns a list of recipient fields ||
|| [mail.recipient.field.get](./recipient/mail-recipient-field-get.md) | Returns a recipient field description ||
|#

## Continue learning

- [{#T}](../index.md)