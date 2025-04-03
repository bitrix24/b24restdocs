# Chat Search: Overview of Methods

With the search feature, you can find group chats, departments, and employees. The search is managed by a group of methods im.search.*.

All methods support standard pagination. You can customize the selection using the `OFFSET` and `LIMIT` parameters.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [Chats in Bitrix24: Design and features](https://helpdesk.bitrix24.com/open/21924784/)

## Find Chat by Name

The method [im.search.chat.list](./im-search-chat-list.md) searches for group chats by name and participants. The search is performed on substrings:
-  in the chat title,
-  first and last names of participants.

## Find Departments

The method [im.search.department.list](./im-search-department-list.md) searches for company departments by their full name. By using the `USER_DATA=Y` parameter, you can include information about the department head in the response.

## Find Employees

The method [im.search.user.list](./im-search-user-list.md) searches for employees by first name, last name, position, and department.

## Overview of Methods {#all-methods}

#|
|| **Method** | **Description** ||
|| [im.search.chat.list](./im-search-chat-list.md) | Searches for chats by names ||
|| [im.search.department.list](./im-search-department-list.md) | Searches for departments ||
|| [im.search.user.list](./im-search-user-list.md) | Searches for users ||
|#

### Methods for the Previous Chat Version

These methods were developed for the previous version of the chat. In the current chat version M1, they work, but the results are not displayed in the interface.

{% note tip "User Documentation" %}

- [Bitrix24 Chat: new messenger and AI](https://helpdesk.bitrix24.com/open/19246004/)

{% endnote %}

#|
|| **Method** | **Description** ||
|| [im.search.last.add](./im-search-last-add.md) | Adds search to history ||
|| [im.search.last.get](./im-search-last-get.md) | Retrieves search history ||
|| [im.search.last.delete](./im-search-last-delete.md) | Deletes search from history ||
|#