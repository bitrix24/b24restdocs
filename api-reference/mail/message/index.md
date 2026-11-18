# E-mails in mail: methods overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

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

{% note info "" %}

The methods in this section belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

## Workflow for Working with E-mails

1. Retrieve a mailbox ID using [mail.mailbox.list](../mailbox/mail-mailbox-list.md) if you need to search for e-mails in a single mailbox
2. Find e-mails using [mail.message.list](./mail-message-list.md)
3. Retrieve the e-mail text and CC recipients using [mail.message.get](./mail-message-get.md), or the correspondence thread using [mail.message.thread](./mail-message-thread.md)
4. Send a new e-mail, a reply, or a forward using [mail.message.send](./mail-message-send.md), [mail.message.reply](./mail-message-reply.md), and [mail.message.forward](./mail-message-forward.md)
5. Move e-mails to a folder, spam, or the trash using [mail.message.movetofolder](./mail-message-movetofolder.md), or create an object in another Bitrix24 tool from the e-mail

## Method Specifics

- Method names are passed in lowercase: `mail.message.createtask`, not `mail.message.createTask`. Bitrix24 does not find a name in any other case
- A user works only with e-mails from mailboxes they have access to. If the user has no mailboxes, `mail.message.list` returns an empty list
- The `create*` and `removecrmactivity` methods accept the e-mail ID `messageId`. It is returned by `mail.message.list` and `mail.message.get`
- You cannot retrieve a list of mailbox folders via REST. The folder name or path for `mail.message.movetofolder` must be known in advance

## Connection with Other Objects

**Mailbox.** An e-mail belongs to a mailbox, whose ID is returned in the `mailboxId` field. To search for e-mails in a single mailbox, pass `mailboxId` to [mail.message.list](./mail-message-list.md). The list of mailboxes is returned by [mail.mailbox.list](../mailbox/mail-mailbox-list.md).

**Sender and Recipients.** The sender address in `from` must be available to the user. The list of available addresses and their `senderId` values is returned by [mail.mailbox.senders](../mailbox/mail-mailbox-senders.md). Recipient addresses can be selected using the methods of the [Recipients](../recipient/index.md) section.

**Bitrix24 Objects.** From an e-mail, you can create a CRM activity, a task, a calendar event, a chat, or a Feed post. The created links are returned in the `bindings` field of the [mail.message.list](./mail-message-list.md) and [mail.message.get](./mail-message-get.md) responses. The methods for each object are listed in the [table](#all-methods). CRM activities created from an e-mail are deleted using the [mail.message.removecrmactivity](./mail-message-removecrmactivity.md) method.


## Overview of Methods {#all-methods}

> Scope: [`mail`](../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

#|
|| **Method** | **Description** ||
|| [mail.message.list](./mail-message-list.md) | Returns a list of e-mails from the current user's mailboxes ||
|| [mail.message.get](./mail-message-get.md) | Returns an e-mail by identifier ||
|| [mail.message.thread](./mail-message-thread.md) | Returns a correspondence thread by the identifier of a single e-mail ||
|| [mail.message.send](./mail-message-send.md) | Sends a new e-mail ||
|| [mail.message.reply](./mail-message-reply.md) | Sends a reply to an e-mail ||
|| [mail.message.forward](./mail-message-forward.md) | Forwards an e-mail ||
|| [mail.message.movetofolder](./mail-message-movetofolder.md) | Moves e-mails to a folder, spam, or trash ||
|| [mail.message.createcrmactivity](./mail-message-createcrmactivity.md) | Creates a CRM activity from an e-mail ||
|| [mail.message.removecrmactivity](./mail-message-removecrmactivity.md) | Deletes CRM activities created from an e-mail ||
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