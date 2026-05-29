# E-mails in mail: methods overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

E-mails in Bitrix24 mail are messages in a user's mailboxes, including correspondence threads and related activities.

The section methods allow you to:
- search for e-mails and retrieve an individual e-mail or a thread of e-mails
- send a new e-mail, a reply, or a forward
- move e-mails to folders, spam, or the trash
- create related objects in other tools from an e-mail
- retrieve a list of e-mail fields and the description of a specific field

> Quick links: [all methods](#all-methods)
>
> User documentation: [Manage emails in Bitrix24](https://helpdesk.bitrix24.com/open/20134658/)

## When to use each method

Use [mail.message.list](./mail-message-list.md) when you need to find e-mails in the current user's mailboxes based on filter conditions.

Use [mail.message.get](./mail-message-get.md) when you need to retrieve an e-mail by its `id`.

Use [mail.message.thread](./mail-message-thread.md) when you need to retrieve an e-mail thread by the `id` of a single e-mail.

Use [mail.message.send](./mail-message-send.md), [mail.message.reply](./mail-message-reply.md), and [mail.message.forward](./mail-message-forward.md) when you need to send a new e-mail, a reply, or a forward.

Use [mail.message.movetofolder](./mail-message-movetofolder.md) when you need to move e-mails to a folder, spam, or the trash.

Use [mail.message.createcrmactivity](./mail-message-createcrmactivity.md) and [mail.message.removecrmactivity](./mail-message-removecrmactivity.md) when you need to create or delete a link between an e-mail and a CRM activity.

Use [mail.message.createtask](./mail-message-createtask.md), [mail.message.createcalendarevent](./mail-message-createcalendarevent.md), [mail.message.createchat](./mail-message-createchat.md), and [mail.message.createfeedpost](./mail-message-createfeedpost.md) when you need to pass the e-mail content to a task, calendar, chat, or Feed.

Use [mail.message.field.list](./mail-message-field-list.md) and [mail.message.field.get](./mail-message-field-get.md) when you need to:
- find out the available e-mail fields
- retrieve the types and metadata of a specific field

## Workflow for working with e-mails

1. Retrieve a list of e-mails via [mail.message.list](./mail-message-list.md)
2. Retrieve full e-mail data via [mail.message.get](./mail-message-get.md) or a thread via [mail.message.thread](./mail-message-thread.md)
3. Perform the target action: send a new e-mail, a reply, or a forward via [mail.message.send](./mail-message-send.md), [mail.message.reply](./mail-message-reply.md), [mail.message.forward](./mail-message-forward.md)
4. If necessary, move e-mails to another folder via [mail.message.movetofolder](./mail-message-movetofolder.md) or create related objects in CRM, tasks, calendar, chat, and Feed
5. Clarify the field structure via `*.field.list` and `*.field.get`

## Section restrictions

- The current user can only work with e-mails from mailboxes they have access to
- Most operations require a correct `id` of an e-mail from `mail.message.list` or `mail.message.get`

## Connection with Other Objects

**Mailbox.** Section methods work with e-mails in the user's mailboxes. A mailbox identifier can be retrieved using [mail.mailbox.*](../mailbox/index.md) methods.

**Recipient.** To select recipients, use [mail.recipient.*](../recipient/index.md) methods.

**CRM.** An e-mail can be linked to a CRM activity, and this link can be removed via [mail.message.createcrmactivity](./mail-message-createcrmactivity.md) and [mail.message.removecrmactivity](./mail-message-removecrmactivity.md) methods.

**Task.** An e-mail can be converted into a task using the [mail.message.createtask](./mail-message-createtask.md) method.

**Calendar.** A calendar event can be created from an e-mail using the [mail.message.createcalendarevent](./mail-message-createcalendarevent.md) method.

**Chat.** The text of an e-mail can be discussed in a chat using the [mail.message.createchat](./mail-message-createchat.md) method.

**News feed.** An e-mail can be published as a post in the News feed using the [mail.message.createfeedpost](./mail-message-createfeedpost.md) method.

## Overview of Methods {#all-methods}

> Scope: [`mail`](../../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

#|
|| **Method** | **Description** ||
|| [mail.message.list](./mail-message-list.md) | Searches for e-mails in the current user's mailboxes ||
|| [mail.message.get](./mail-message-get.md) | Returns an e-mail by identifier ||
|| [mail.message.thread](./mail-message-thread.md) | Returns an e-mail thread by the identifier of a single e-mail ||
|| [mail.message.send](./mail-message-send.md) | Sends a new e-mail ||
|| [mail.message.reply](./mail-message-reply.md) | Sends a reply to an e-mail ||
|| [mail.message.forward](./mail-message-forward.md) | Forwards an e-mail ||
|| [mail.message.movetofolder](./mail-message-movetofolder.md) | Moves e-mails to a folder, spam, or trash ||
|| [mail.message.createcrmactivity](./mail-message-createcrmactivity.md) | Creates a CRM activity from an e-mail ||
|| [mail.message.removecrmactivity](./mail-message-removecrmactivity.md) | Removes the link between an e-mail and a CRM activity ||
|| [mail.message.createtask](./mail-message-createtask.md) | Creates a task from an e-mail ||
|| [mail.message.createcalendarevent](./mail-message-createcalendarevent.md) | Creates a calendar event from an e-mail ||
|| [mail.message.createchat](./mail-message-createchat.md) | Creates a chat from an e-mail ||
|| [mail.message.createfeedpost](./mail-message-createfeedpost.md) | Creates a News Feed post from an e-mail ||
|| [mail.message.field.list](./mail-message-field-list.md) | Returns a list of e-mail fields ||
|| [mail.message.field.get](./mail-message-field-get.md) | Returns the description of an e-mail field ||
|#

## Continue Learning

- [{#T}](../mailbox/index.md)
- [{#T}](../recipient/index.md)
- [{#T}](../index.md)