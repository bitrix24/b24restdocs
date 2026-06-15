# Company Structure in REST 3.0: Overview of Sections

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The company structure illustrates the departments that make up the organization and how they are interconnected. Each department and team can define the roles of participants: leader, deputy, and staff members. This helps maintain an up-to-date organizational chart, quickly locate the necessary department or employee, configure collaborative work, and utilize the structure in related Bitrix24 tools.

The methods in this section work with several groups of objects:

- [Departments and Teams](./node/index.md) — create departments and teams, retrieve their list, data, child elements, and field descriptions.
- [Department and Team Members](./node-member/index.md) — add users, set roles, transfer and remove participants.
- [Department and Team Communications](./node-communication/index.md) — retrieve and modify related chats, channels, and collabs.
- [Employees](./employee/index.md) — search for employees, retrieve counters and data about subordinates or part-timers.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Company structure: New interface and features](https://helpdesk.bitrix24.com/open/23583540/)

## Getting Started

1. Retrieve the list of departments or teams through [humanresources.node.list](./node/humanresources-node-list.md) to determine the required `id`.
2. Check the data of the department or team through [humanresources.node.get](./node/humanresources-node-get.md) if the identifier is already known.
3. Create a new department or team through [humanresources.node.add](./node/humanresources-node-add.md) or modify their properties through [humanresources.node.edit](./node/humanresources-node-edit.md).
4. Obtain user identifiers using the [user.get](../../user/user-get.md) method if you need to prepare the participant list.
5. Add participants through [humanresources.node.member.add](./node-member/humanresources-node-member-add.md) or set the full composition through [humanresources.node.member.set](./node-member/humanresources-node-member-set.md).
6. Configure related chats, channels, and collabs using the [humanresources.node.communication.edit](./node-communication/humanresources-node-communication-edit.md) method if separate communications are needed for the department or team.
7. Find employees using the [humanresources.employee.search](./employee/humanresources-employee-search.md) method if you need to obtain employee data, check subordinates, or find employees across multiple departments.

## Limitations and Recommendations

- Access to view and modify the company structure depends on the current user's permissions.
- Different sets of participant roles are used for departments and teams.
- Use field description methods to check available fields and their types before modifying data.

{% note tip "User Documentation" %}

- [Create and change departments in the company structure](https://helpdesk.bitrix24.com/open/23599408/)
- [Cross-Functional Teams in Bitrix24](https://helpdesk.bitrix24.com/open/25609471/)
- [Access permissions to the company structure](https://helpdesk.bitrix24.com/open/23580546/)

{% endnote %}

## Connection with Other Objects

**Users.** To add, transfer, and remove participants, user identifiers `userIds` are needed. These can be obtained using the [user.get](../../user/user-get.md) method.

**Chats, Channels, and Collabs.** When creating a department or team, the [humanresources.node.add](./node/humanresources-node-add.md) method allows you to create or link related chats, channels, and collabs immediately. After establishing the connection, you can retrieve it using the [humanresources.node.communication.list](./node-communication/humanresources-node-communication-list.md) method and modify it using the [humanresources.node.communication.edit](./node-communication/humanresources-node-communication-edit.md) method.

**Employees.** The methods [humanresources.employee.search](./employee/humanresources-employee-search.md), [humanresources.employee.subordinates](./employee/humanresources-employee-subordinates.md), and [humanresources.employee.multidepartment](./employee/humanresources-employee-multidepartment.md) help find users in the company structure and check their connections with departments.

## Overview of Methods {#all-methods}

> Scope: [`humanresources`](../../scopes/permissions.md)
>
> Who can execute methods: depends on the method

### Departments and Teams

#| 
|| **Method** | **Description** ||
|| [humanresources.node.add](./node/humanresources-node-add.md) | Creates a department or team ||
|| [humanresources.node.edit](./node/humanresources-node-edit.md) | Updates fields of a department or team ||
|| [humanresources.node.get](./node/humanresources-node-get.md) | Returns a department or team by identifier ||
|| [humanresources.node.list](./node/humanresources-node-list.md) | Returns a list of departments and teams ||
|| [humanresources.node.search](./node/humanresources-node-search.md) | Searches for departments and teams by name ||
|| [humanresources.node.children](./node/humanresources-node-children.md) | Returns child departments and teams ||
|| [humanresources.node.count](./node/humanresources-node-count.md) | Returns the number of departments and teams ||
|| [humanresources.node.move](./node/humanresources-node-move.md) | Moves a department or team to a new parent ||
|| [humanresources.node.field.list](./node/humanresources-node-field-list.md) | Returns a list of fields for a department or team ||
|| [humanresources.node.field.get](./node/humanresources-node-field-get.md) | Returns the description of a field for a department or team ||
|#

### Department and Team Members

#| 
|| **Method** | **Description** ||
|| [humanresources.node.member.add](./node-member/humanresources-node-member-add.md) | Adds users to a department or team ||
|| [humanresources.node.member.set](./node-member/humanresources-node-member-set.md) | Updates the composition of department or team members by roles ||
|| [humanresources.node.member.move](./node-member/humanresources-node-member-move.md) | Transfers users to another department or team ||
|| [humanresources.node.member.remove](./node-member/humanresources-node-member-remove.md) | Removes users from a department or team ||
|#

### Department and Team Communications

#|
|| **Method** | **Description** ||
|| [humanresources.node.communication.edit](./node-communication/humanresources-node-communication-edit.md) | Modifies related chats, channels, or collabs of a department or team ||
|| [humanresources.node.communication.list](./node-communication/humanresources-node-communication-list.md) | Returns related chats, channels, and collabs of a department or team ||
|#

### Employees

#|
|| **Method** | **Description** ||
|| [humanresources.employee.search](./employee/humanresources-employee-search.md) | Searches for employees by name ||
|| [humanresources.employee.subordinates](./employee/humanresources-employee-subordinates.md) | Returns subordinates of a user by departments ||
|| [humanresources.employee.count](./employee/humanresources-employee-count.md) | Returns the number of employees in the company structure ||
|| [humanresources.employee.multidepartment](./employee/humanresources-employee-multidepartment.md) | Returns employees who belong to multiple departments ||
|| [humanresources.employee.field.list](./employee/humanresources-employee-field-list.md) | Returns a list of fields for an employee ||
|| [humanresources.employee.field.get](./employee/humanresources-employee-field-get.md) | Returns the description of a field for an employee ||
|#

## Continue Learning

- [{#T}](./node/index.md)
- [{#T}](./node-member/index.md)
- [{#T}](./node-communication/index.md)
- [{#T}](./employee/index.md)
- [{#T}](../index.md)
