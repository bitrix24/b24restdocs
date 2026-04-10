# Attached File: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The attached file links a document from Drive to other Bitrix24 objects. This connection allows access to the file from various modules within the system.

> Quick Navigation: [All Methods](#all-methods)

## Features of Working with Attached Files

The method `disk.attachedObject.get` operates on the record of attaching a file to a specific object, rather than the file itself. Imagine you have a file `report.docx` in Drive. When you attach it to a task and a comment in the feed, two different attachment records are created with different identifiers, but both reference the same file.

To obtain the connection identifier, you need to use methods that return attached files within specific objects. For example, when working with tasks, the connection identifier can be retrieved through the method [tasks.task.get](../../tasks/tasks-task-get.md).

## Connection to Other Objects

**CRM.** Files can be attached to deals and estimates. The group of methods [crm.activity.*](../../crm/timeline/activities/index.md) is responsible for working with deals, while [crm.quote.*](../../crm/quote/index.md) handles estimates.

**Workflows.** You can initiate workflows for documents in the shared drive. Workflow management is performed using the methods [bizproc.workflow.*](../../bizproc/index.md).

**Tasks.** Files are attached to the task description. All Participants in the task can view, edit, and download the attached files. You should work with tasks through the methods in the group [tasks.task.*](../../tasks/index.md).

**Calendar.** Files are added to events and become accessible to all participants. You can retrieve information about an event using the methods [calendar.event.*](../../calendar/index.md).

**News Feed.** Files are attached to messages and comments. The method [log.blogpost.get](../../log/log-blogpost-get.md) returns a list of messages, while the method [log.blogcomment.user.get](../../log/blogcomment/log-blogcomment-user-get.md) returns a list of comments.

**E-mail.** Attachments from e-mails are saved to Drive. You can only work with e-mails through the Bitrix24 interface. If an e-mail is saved in CRM, you can interact with the attachments using the methods [crm.activity.*](../../crm/timeline/activities/index.md) through the `FILES` field.

**Universal Lists.** List items are linked to Drive through the [field](../../lists/fields/index.md) of type File (Drive). You can retrieve the parameters of an item using the method [lists.element.get](../../lists/elements/lists-element-get.md).

**Chats.** Users can exchange documents, photos, videos, and audio files. Files can be viewed and downloaded, while documents can be edited without downloading. Attached files are accessible to all chat participants. The method [im.dialog.messages.get](../../chats/messages/im-dialog-messages-get.md) returns a list of the latest messages.

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Read" access permission for the required file

#|
|| **Method** | **Description** ||
|| [disk.attachedObject.get](./disk-attached-object-get.md) | Returns information about the attached file ||
|#