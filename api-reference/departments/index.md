# Company Structure: Overview of Methods

The company structure illustrates the hierarchy of departments that make up the organization. Each department lists its leaders, deputies, and subordinates.

Technically, the company structure is an information block, and the departments are sections of that information block. You can obtain a list of departments using the [department.get](department-get.md) method.

> Quick navigation: [all methods](#all-methods) 
>
> User documentation: [Company structure](https://helpdesk.bitrix24.com/open/18082492/)

## Connection of Departments with Other Objects

**User**. Leaders are linked to departments by a numerical identifier in the `UF_HEAD` parameter. You can obtain the user identifier using the [user.get](../user/user-get.md) method.

## Actions with Departments

Company departments can be:

- created using the [department.add](department-add.md) method
- modified, including being moved, using the [department.update](department-update.md) method
- deleted using the [department.delete](department-delete.md) method

## Overview of Methods {#all-methods}

> Scope: [`department`](../scopes/permissions.md)
>
> Who can perform the method: depending on the method

#|
|| **Method** | **Description** ||
|| [department.add](department-add.md) | Create a department ||
|| [department.update](department-update.md) | Modify a department ||
|| [department.get](department-get.md) | Retrieve a list of departments ||
|| [department.fields](department-fields.md) | Get the department fields reference ||
|| [department.delete](department-delete.md) | Delete a department ||
|#