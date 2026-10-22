# Forum in REST 3.0: Overview of Sections

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The forum stores the messages that make up discussions in Bitrix24: comments on calendar events, work time reports, photos, wiki pages, and universal list items, as well as forum posts. Task comments are stored in the task chat, not in the forum.

The section methods work with two groups of objects:

- [Messages](./message/index.md) — delete a forum message
- [Message Custom Fields](./userfield/index.md) — create, update, retrieve, and delete custom fields of forum messages, and manage the values of "List" fields

> Quick navigation: [all methods](#all-methods)

{% note info "" %}

The methods in this section belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../rest-v3.md).

{% endnote %}

## Getting Started

1. Retrieve the custom fields of messages with [forum.message.field.custom.list](./userfield/forum-message-field-custom-list.md), or create a new field with [forum.message.field.custom.add](./userfield/forum-message-field-custom-add.md)
2. For a "List" field, add value options with [forum.message.field.custom.enum.add](./userfield/forum-message-field-custom-enum-add.md)
3. To delete a comment or a forum post, call [forum.message.delete](./message/forum-message-delete.md) with the message identifier

## Limitations and Features

- REST 3.0 has no method that returns forum messages, so the identifier of the message to delete must be obtained outside the section methods
- A message can be deleted by its author or an administrator. The related News Feed post or comment is deleted together with the message
- The specifics of custom fields and list values are described in the [custom fields overview](./userfield/index.md#restrictions)

## Connection with Other Objects

**Calendar.** A comment on a calendar event is stored as a forum message. Calendar events are created and updated with the [calendar.event.*](../calendar/calendar-event/index.md) methods. The [forum.message.delete](./message/forum-message-delete.md) method deletes such a comment by its identifier.

**Tasks.** A task comment is a message in the task chat, and the forum methods do not work with it. The methods for comments are described in the [task comments overview](../tasks/comment-item/index.md).

**News Feed.** Comments on calendar events, work time reports, and other objects are shown in the [News Feed](../log/index.md). When a message is deleted with [forum.message.delete](./message/forum-message-delete.md), the comment also disappears from the News Feed.

## Overview of Methods {#all-methods}

> Scope: [`forum`](../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

### Messages

#|
|| **Method** | **Description** ||
|| [forum.message.delete](./message/forum-message-delete.md) | Deletes a forum message ||
|#

### Message Custom Fields

#|
|| **Method** | **Description** ||
|| [forum.message.field.custom.add](./userfield/forum-message-field-custom-add.md) | Adds a custom field to forum messages ||
|| [forum.message.field.custom.update](./userfield/forum-message-field-custom-update.md) | Updates a custom field ||
|| [forum.message.field.custom.get](./userfield/forum-message-field-custom-get.md) | Returns a custom field by its identifier ||
|| [forum.message.field.custom.list](./userfield/forum-message-field-custom-list.md) | Returns the list of custom fields of forum messages ||
|| [forum.message.field.custom.delete](./userfield/forum-message-field-custom-delete.md) | Deletes a custom field ||
|#

### Values of "List" Fields

#|
|| **Method** | **Description** ||
|| [forum.message.field.custom.enum.add](./userfield/forum-message-field-custom-enum-add.md) | Adds a value to a "List" field ||
|| [forum.message.field.custom.enum.update](./userfield/forum-message-field-custom-enum-update.md) | Updates a list value ||
|| [forum.message.field.custom.enum.get](./userfield/forum-message-field-custom-enum-get.md) | Returns a list value by its identifier ||
|| [forum.message.field.custom.enum.list](./userfield/forum-message-field-custom-enum-list.md) | Returns the values of a "List" field ||
|| [forum.message.field.custom.enum.delete](./userfield/forum-message-field-custom-enum-delete.md) | Deletes a list value ||
|#

## Continue Learning

- [{#T}](./message/index.md)
- [{#T}](./userfield/index.md)
- [{#T}](../rest-v3.md)
