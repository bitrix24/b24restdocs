# Chat Search: Overview of Methods

The search functionality allows you to find group chats, departments, and employees. The search is managed by a group of methods `im.search.*`.

All methods in the current version of the chat support standard pagination. You can customize the selection using the `OFFSET` and `LIMIT` parameters.

> Quick navigation: [all methods and events](#all-methods)  
> User documentation: [Chats in Bitrix24: Design and features](https://helpdesk.bitrix24.com/open/21924784/)

## Find Chat by Name

The method [im.search.chat.list](./im-search-chat-list.md) searches for group chats by name and participants. The search is performed on substrings:
- in the chat title,
- in the first and last names of participants.

## Find Departments

The method [im.search.department.list](./im-search-department-list.md) searches for company departments by their full name. By using the `USER_DATA=Y` parameter, you can include information about the department head in the response.

## Find Employees

The method [im.search.user.list](./im-search-user-list.md) searches for employees by first name, last name, position, and department.

## Overview of Methods {#all-methods}

> Scope: [`im`](../../scopes/permissions.md)  
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [im.search.chat.list](./im-search-chat-list.md) | Searches for chats by names ||
|| [im.search.department.list](./im-search-department-list.md) | Searches for departments ||
|| [im.search.user.list](./im-search-user-list.md) | Searches for users ||
|#

### Methods for the Previous Version of the Chat

These methods were designed for the previous version of the chat. In the current version, M1, they still function, but the results are not displayed in the interface.

{% note tip "User Documentation" %}

- [Bitrix24 Chat: new messenger](https://helpdesk.bitrix24.com/open/19246004/)

{% endnote %}

#|
|| **Method** | **Description** ||
|| [im.search.last.add](./im-search-last-add.md) | Adds search to history ||
|| [im.search.last.get](./im-search-last-get.md) | Retrieves search history ||
|| [im.search.last.delete](./im-search-last-delete.md) | Deletes search from history ||
|#