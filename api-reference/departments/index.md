# Company Structure: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The company structure shows which departments make up the company and how they are connected. In departments and teams, you can define participant roles: head, deputy, and employees.

In [REST 3.0](../rest-v3.md), the company structure is described by `humanresources.*` methods. They work with departments and teams, participants and roles, connected chats, channels, and collabs, employee search, and field schema methods (`*.field.list` / `*.field.get`). `department.*` methods belong to the previous API version and work with departments.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Company structure](https://helpdesk.bitrix24.com/open/18082492/)

## How to Choose a Method Group

#|
|| **If you need to** | **Open** ||
|| Work with departments, teams, participants, roles, and connected communications | `humanresources.*` REST 3.0 methods ||
|| Work with departments from the previous API version | `department.*` methods ||
|#

## Getting Started

### If You Work with REST v2

1. Get a list of departments using [department.get](department-get.md) to identify the required `ID`
2. Check department fields using [department.fields](department-fields.md) if you need to prepare data for creation or update
3. Get the head's identifier using [user.get](../user/user-get.md) if you need to fill in the `UF_HEAD` field
4. Create a department using [department.add](department-add.md) or update it using [department.update](department-update.md)
5. Delete a department using [department.delete](department-delete.md) if it is no longer needed

### If You Work with REST 3.0

1. Get a list of departments or teams using [humanresources.node.list](./node/humanresources-node-list.md) to identify the required `id`
2. Check department or team data using [humanresources.node.get](./node/humanresources-node-get.md) if the identifier is already known
3. Create a new department or team using [humanresources.node.add](./node/humanresources-node-add.md), or update its properties using [humanresources.node.edit](./node/humanresources-node-edit.md)
4. Get user identifiers using [user.get](../user/user-get.md) if you need to prepare the participant list
5. Add participants using [humanresources.node.member.add](./node-member/humanresources-node-member-add.md) or set the full participant list using [humanresources.node.member.set](./node-member/humanresources-node-member-set.md)
6. Configure connected chats, channels, and collabs using [humanresources.node.communication.edit](./node-communication/humanresources-node-communication-edit.md) if the department or team needs separate communications
7. Find employees using [humanresources.employee.search](./employee/humanresources-employee-search.md) if you need to get employee data, check subordinates, or find employees in multiple departments

## Limitations and Recommendations

- Access to viewing and changing the company structure depends on the current user's permissions
- Departments and teams use different sets of participant roles
- Field description methods help check available fields and their types before changing data

{% note tip "User documentation" %}

- [Company structure](https://helpdesk.bitrix24.com/open/18082492/)

{% endnote %}

## Connection with Other Objects

**Users.** In `department.*` methods, heads are linked to departments by a numeric identifier in the `UF_HEAD` parameter. In `humanresources.*` methods, user identifiers `userIds` are required to add, move, and remove participants. Identifiers can be obtained using [user.get](../user/user-get.md).

**Chats, channels, and collabs.** When creating a department or team, [humanresources.node.add](./node/humanresources-node-add.md) lets you immediately create or bind connected chats, channels, and collabs. After creation, the connection can be retrieved using [humanresources.node.communication.list](./node-communication/humanresources-node-communication-list.md) and changed using [humanresources.node.communication.edit](./node-communication/humanresources-node-communication-edit.md).

**Employees.** The methods [humanresources.employee.search](./employee/humanresources-employee-search.md), [humanresources.employee.subordinates](./employee/humanresources-employee-subordinates.md), and [humanresources.employee.multidepartment](./employee/humanresources-employee-multidepartment.md) help find users in the company structure and check their connection with departments.

## Overview of Methods {#all-methods}

### REST v2 Methods

> Scope: [`department`](../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [department.add](department-add.md) | Creates a department ||
|| [department.update](department-update.md) | Updates a department ||
|| [department.get](department-get.md) | Returns a list of departments ||
|| [department.fields](department-fields.md) | Returns the department fields reference ||
|| [department.delete](department-delete.md) | Deletes a department ||
|#

### REST 3.0 Methods

> Scope: [`humanresources`](../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

#### Departments and Teams

#|
|| **Method** | **Description** ||
|| [humanresources.node.add](./node/humanresources-node-add.md) | Creates a department or team ||
|| [humanresources.node.edit](./node/humanresources-node-edit.md) | Updates department or team fields ||
|| [humanresources.node.get](./node/humanresources-node-get.md) | Returns a department or team by identifier ||
|| [humanresources.node.list](./node/humanresources-node-list.md) | Returns a list of departments and teams ||
|| [humanresources.node.search](./node/humanresources-node-search.md) | Searches departments and teams by name ||
|| [humanresources.node.children](./node/humanresources-node-children.md) | Returns child departments and teams ||
|| [humanresources.node.count](./node/humanresources-node-count.md) | Returns the number of departments and teams ||
|| [humanresources.node.move](./node/humanresources-node-move.md) | Moves a department or team to a new parent ||
|| [humanresources.node.field.list](./node/humanresources-node-field-list.md) | Returns a list of department or team fields ||
|| [humanresources.node.field.get](./node/humanresources-node-field-get.md) | Returns the description of a department or team field ||
|#

#### Department and Team Participants

#|
|| **Method** | **Description** ||
|| [humanresources.node.member.add](./node-member/humanresources-node-member-add.md) | Adds users to a department or team ||
|| [humanresources.node.member.set](./node-member/humanresources-node-member-set.md) | Updates department or team participants by role ||
|| [humanresources.node.member.move](./node-member/humanresources-node-member-move.md) | Moves users to another department or team ||
|| [humanresources.node.member.remove](./node-member/humanresources-node-member-remove.md) | Removes users from a department or team ||
|| [humanresources.node.member.field.list](./node-member/humanresources-node-member-field-list.md) | Returns a list of department or team participant fields ||
|| [humanresources.node.member.field.get](./node-member/humanresources-node-member-field-get.md) | Returns the description of a department or team participant field ||
|#

#### Department and Team Communications

#|
|| **Method** | **Description** ||
|| [humanresources.node.communication.edit](./node-communication/humanresources-node-communication-edit.md) | Changes connected chats, channels, or collabs of a department or team ||
|| [humanresources.node.communication.list](./node-communication/humanresources-node-communication-list.md) | Returns connected chats, channels, and collabs of a department or team ||
|#

#### Employees

#|
|| **Method** | **Description** ||
|| [humanresources.employee.search](./employee/humanresources-employee-search.md) | Searches employees by name ||
|| [humanresources.employee.subordinates](./employee/humanresources-employee-subordinates.md) | Returns a user's subordinates by department ||
|| [humanresources.employee.count](./employee/humanresources-employee-count.md) | Returns the number of employees in the company structure ||
|| [humanresources.employee.multidepartment](./employee/humanresources-employee-multidepartment.md) | Returns employees who belong to multiple departments ||
|| [humanresources.employee.field.list](./employee/humanresources-employee-field-list.md) | Returns a list of employee fields ||
|| [humanresources.employee.field.get](./employee/humanresources-employee-field-get.md) | Returns the description of an employee field ||
|#
