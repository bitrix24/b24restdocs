# Working with Universal Lists: Overview of Methods

Universal lists can be used to keep track of any objects: documents, certificates, promotional materials, and more.

Only a Bitrix24 administrator can create lists. They also configure the permissions: specifying who among the employees can edit the list and who does not have access to it.

> Quick navigation: [all methods](#all-methods) 
>
> User documentation: [Example of working with a list](https://helpdesk.bitrix24.com/open/16705502/)

## Types of Information Blocks for Lists

In Bitrix24, three types of information blocks are used for lists:

-  `lists` — standard universal lists,
-  `lists_socnet` — lists for workgroups and projects,
-  `bitrix_processes` — a service type for business process data.

To find out the type of an existing list, use the method [lists.get.iblock.type.id](./lists-get-iblock-type-id.md).

## Workgroups and Projects

Universal lists are integrated [into workgroups and projects](../../sonet-group/sonet-group-create.md). You can create your lists within any group or project with various access levels for participants.

{% note tip "User documentation" %}

- [Create a list in a workgroup or project](https://helpdesk.bitrix24.com/open/25749275/)

{% endnote %}

## Business Processes

Business processes in universal lists create a custom scenario for processing elements. For example, a list item contains the expiration date of a certificate. When such an item is added, a business process is triggered. N days before the expiration date, it will automatically create a task for the responsible employee.

Business process management is performed using the group of methods [bizproc.workflow.*](../../bizproc/index.md) and [bizproc.task.*](../../bizproc/bizproc-task/bizproc-task-list.md).

{% note tip "User documentation" %}

-  [Enable workflows in Lists](https://helpdesk.bitrix24.com/open/8440151/)

{% endnote %}

## Access Permissions

Permissions for the entire list are configured for users, groups, and departments. Set them when creating or updating the list through the `RIGHTS` parameter. In this parameter, specify an array where the key is the entity code, for example, `U{ID}` for a user, and the value is the letter code of the permission, for example, `W` for write access.

Permissions can conflict; for example, a user has a role with certain permissions, while the department they belong to has different ones. In this case, the maximum permissions are applied.

## Overview of Methods {#all-methods}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [lists.add](./lists-add.md) | Creates a universal list ||
|| [lists.update](./lists-update.md) | Updates a universal list ||
|| [lists.get](./lists-get.md) | Returns data of a universal list or an array of lists ||
|| [lists.delete](./lists-delete.md) | Deletes a universal list ||
|| [lists.get.iblock.type.id](./lists-get-iblock-type-id.md) | Returns the identifier of the information block type ||
|#