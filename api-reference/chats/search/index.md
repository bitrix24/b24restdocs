# Chat Search: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The `im.search.*` method group finds group chats, departments, and employees by a search phrase. The subsection is part of the [Chats in Bitrix24](../index.md) section.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Chats in Bitrix24: Design and features](https://helpdesk.bitrix24.com/open/21924784/)

## What to Search For and With Which Method

#|
|| **What You Need to Find** | **Method** | **How It Searches** ||
|| A group chat | [im.search.chat.list](./im-search-chat-list.md) | By the beginning of words in the chat title and in the names of participants. The separate `FIND_LINES` parameter searches Open Channel chats ||
|| A company department | [im.search.department.list](./im-search-department-list.md) | By the beginning of words in the full name of the department, including the names of the parent departments ||
|| An employee | [im.search.user.list](./im-search-user-list.md) | By the beginning of words in the first name, last name, position, and department ||
|| A message inside a chat | [im.dialog.messages.search](../messages/im-dialog-messages-search.md) | By the message text within one chat. A method of the [Messages](../messages/index.md) subsection ||
|| A notification in the history | [im.notify.history.search](../notifications/im-notify-history-search.md) | By the text, type, date, and tag of the notification. A method of the [Notifications](../notifications/index.md) subsection ||
|| A chat in the recent list | [im.recent.list](../im-recent-list.md) | Without a search phrase: the method returns the whole list of recent chats ||
|#

## Limits and Response Format

The rules below apply to the three search methods — `im.search.chat.list`, `im.search.department.list`, and `im.search.user.list`. The search history methods `im.search.last.*` have neither a search phrase nor pagination.

- the search runs against the beginning of words, not an arbitrary substring: "Proj" finds "Project Chat", while "roject" does not
- [im.search.chat.list](./im-search-chat-list.md) and [im.search.user.list](./im-search-user-list.md) require at least two characters: for a shorter phrase they return the `FIND_SHORT` error
- [im.search.department.list](./im-search-department-list.md) behaves differently: it returns the `FIND_SHORT` error only if the `FIND` parameter is not provided at all. An empty or very short phrase switches the filter off, and the method returns the whole list of departments — check the phrase length on your side
- when both `FIND` and `FIND_LINES` are provided, [im.search.chat.list](./im-search-chat-list.md) gives priority to `FIND` and does not search Open Channels
- the selection is paginated: the offset is set by `OFFSET`, and the page size by `LIMIT`. The response contains `total`, and `next` if there is a next page
- the response fields are returned in `snake_case`
- the response shapes differ: [im.search.chat.list](./im-search-chat-list.md) and [im.search.department.list](./im-search-department-list.md) return `result` as an array, while [im.search.user.list](./im-search-user-list.md) returns an object where the key of each element equals the user identifier. For all three, an empty selection comes back as an empty array

## Relationship with Other Objects

**Chat.** The search method returns the `id` of a chat. Substitute it into `DIALOG_ID` in the `chatXXX` format to send a message with the [im.message.add](../messages/im-message-add.md) method, or into `CHAT_ID` of the methods of the [Chat Participants](../chat-users/index.md) subsection. The data of the found chat is returned by [im.dialog.get](../im-dialog-get.md).

**User.** The found `id` of an employee is suitable for a private dialogue: pass it into `DIALOG_ID` as a number. The same identifier is accepted by the methods of the [Users in Chats](../users/index.md) subsection and by the `USERS` parameter of the [im.chat.user.add](../chat-users/im-chat-user-add.md) method.

**Department.** The found `id` of a department is accepted by the methods of the [Departments](../departments/index.md) subsection — they return the managers and employees of the department. The identifier is passed as an array: `ID: [107]`.

## Overview of Methods {#all-methods}

> Scope: [`im`](../../scopes/permissions.md)  
> Who can execute the method: any user

### Current Methods

The methods are recommended for new development.

#|
|| **Method** | **Description** ||
|| [im.search.chat.list](./im-search-chat-list.md) | Searches for chats by names ||
|| [im.search.department.list](./im-search-department-list.md) | Searches for departments ||
|| [im.search.user.list](./im-search-user-list.md) | Searches for users ||
|#

### Deprecated Methods

The `im.search.last.*` methods store the history of a user's search queries. They are deprecated: designed for the previous version of the chat and kept only to support existing integrations. In the current M1 chat version, the methods work, but the results are not displayed in the interface — more about the current version in the article [Bitrix24 Chat: new messenger](https://helpdesk.bitrix24.com/open/19246004/). There is no replacement for them.

#|
|| **Method** | **Description** ||
|| [im.search.last.add](./im-search-last-add.md) | Adds search to history ||
|| [im.search.last.get](./im-search-last-get.md) | Retrieves search history ||
|| [im.search.last.delete](./im-search-last-delete.md) | Deletes search from history ||
|#