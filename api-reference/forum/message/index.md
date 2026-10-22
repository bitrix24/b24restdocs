# Forum Messages: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A forum message is a comment on a calendar event, work time report, photo, wiki page, or universal list item, as well as a forum post. The `forum.message.delete` method deletes a message by its identifier.

> Quick navigation: [all methods](#all-methods)

{% note info "" %}

The method in this section belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

## Message Custom Fields

Custom fields of messages are created and configured with the [forum.message.field.custom.*](../userfield/index.md) methods. The full list of custom fields is returned by [forum.message.field.custom.list](../userfield/forum-message-field-custom-list.md).

## Deleting a Message

The [forum.message.delete](./forum-message-delete.md) method deletes a message by its identifier:

- a message can be deleted by its author or an administrator
- the related post or comment in the [News Feed](../../log/index.md) is deleted together with the message
- for a non-existent identifier, the method also returns `true`, so the response does not confirm that the message existed

REST 3.0 has no method that returns forum messages, so the message identifier must be obtained outside the section methods. Task comments are stored in the task chat, not in the forum, and are not deleted by this method.

## Overview of Methods {#all-methods}

> Scope: [`forum`](../../scopes/permissions.md)
>
> Who can execute the method: the message author or an administrator

#|
|| **Method** | **Description** ||
|| [forum.message.delete](./forum-message-delete.md) | Deletes a forum message ||
|#

## Continue Learning

- [{#T}](./forum-message-delete.md)
- [{#T}](../userfield/index.md)
- [{#T}](../index.md)
