# Departments in Chats: Overview of Methods

The methods `im.department.*` retrieve information about users in departments from the company's structure. They only work for company employees.

> Quick navigation: [all methods and events](#all-methods)

## Connection of Methods with Other Objects

**Department.** The methods obtain information about users by the department identifier `ID`. You can get the department identifier using the [get department list method](../../departments/department-get.md) or the [search departments by name method](../search/im-search-department-list.md).

## Get Department Data

The method [im.department.get](./im-department-get.md) retrieves the department name and the identifier of the manager. By using the parameter `USER_DATA = 'Y'`, you can include data about the department manager in the response.

## Get User List

You can obtain lists of user identifiers using the following methods:

-  [im.department.managers.get](./im-department-managers-get.md) — managers of the specified departments,
-  [im.department.employees.get](./im-department-employees-get.md) — employees of the specified departments,
-  [im.department.colleagues.list](./im-department-colleagues-list.md) — list of colleagues of the current user. For a manager, the method will return a list of subordinates and all managers.

By using the parameter `USER_DATA = 'Y'`, you can get detailed information about each employee.

## Overview of Methods {#all-methods}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [im.department.get](./im-department-get.md) | Retrieves information about the department ||
|| [im.department.managers.get](./im-department-managers-get.md) | Retrieves a list of department managers ||
|| [im.department.employees.get](./im-department-employees-get.md) | Retrieves a list of department employees ||
|| [im.department.colleagues.list](./im-department-colleagues-list.md) | Retrieves a list of colleagues of the current user ||
|#