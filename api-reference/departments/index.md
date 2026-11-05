# Company Structure: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The company structure shows which departments make up the company and which departments belong to others. Besides departments, the structure contains teams, which are stored in the same tree. In departments and teams, you can assign roles to participants: head, deputy, and employees.

Two groups of methods work with the company structure:

- `humanresources.*` — [REST 3.0](../rest-v3.md) methods for departments and teams, their participants and roles, chats, channels, and collabs, as well as for employee search
- `department.*` — methods of the previous API version, REST v2, only for departments and their heads

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Company structure](https://helpdesk.bitrix24.com/open/18082492/)

{% note info "" %}

`humanresources.*` methods are called at an address that includes `/rest/api/`. The PHP SDK, BX24.js, and PHP CRest do not support this address yet — use direct HTTP requests. See the [REST 3.0 overview](../rest-v3.md#sdk) for information on SDK support.

{% endnote %}

## How to Choose a Method Group

Both groups of methods are available. For a new integration, use REST 3.0 methods: they work with both teams and participants. The `department.*` methods are needed to delete a department: REST 3.0 has no such method. Choose the section based on your task:

#|
|| **If you need to** | **Open the section** ||
|| Create departments and teams, search them, and build a hierarchy | [Departments and Teams](./node/index.md) ||
|| Manage the list of participants and their roles | [Participants of Departments and Teams](./node-member/index.md) ||
|| Configure chats, channels, and collabs of a department or team | [Department and Team Communications](./node-communication/index.md) ||
|| Find an employee, find out how many subordinates they have, or retrieve employees who belong to multiple departments | [Employees](./employee/index.md) ||
|| Work with departments through the previous API version | [`department.*` methods](#all-methods) ||
|#

### Correspondence of REST v2 and REST 3.0 Methods {#v2-v3}

If your integration already runs on `department.*` methods, find a REST 3.0 replacement in the table.

#|
|| **REST v2 method** | **REST 3.0 method** ||
|| [department.add](department-add.md) | [humanresources.node.add](./node/humanresources-node-add.md) ||
|| [department.update](department-update.md) | [humanresources.node.edit](./node/humanresources-node-edit.md) — name and description, [humanresources.node.move](./node/humanresources-node-move.md) — move to another parent department, [participant methods](./node-member/index.md) — head ||
|| [department.get](department-get.md) | [humanresources.node.list](./node/humanresources-node-list.md), [humanresources.node.get](./node/humanresources-node-get.md) ||
|| [department.delete](department-delete.md) | There is no counterpart: a department or team cannot be deleted with REST 3.0 methods ||
|| [department.fields](department-fields.md) | [humanresources.node.field.list](./node/humanresources-node-field-list.md), [humanresources.node.field.get](./node/humanresources-node-field-get.md) ||
|#

## How to Call a REST 3.0 Method

The example calls the `humanresources.node.list` method. REST 3.0 methods are called at the `/rest/api/` address, without the `.json` suffix. The `department.*` methods are called at the `/rest/` address, without `api/`.

```bash
curl -X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-d '{"type":"DEPARTMENT","pagination":{"limit":2}}' \
https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/humanresources.node.list
```

The list is returned in `result.items`; the response has no `total` field. The example returns only the first two departments. How to retrieve the next ones is described for the `pagination` parameter on the [humanresources.node.list](./node/humanresources-node-list.md) page, along with a full response example. The REST 3.0 response and error format is described in the article [{#T}](../rest-v3.md).

## Getting Started

### If You Work with REST v2

1. Get a list of departments using [department.get](department-get.md) to identify the required `ID`
2. Check department fields using [department.fields](department-fields.md) if you need to prepare data for creation or update
3. Get the head's identifier using [user.get](../user/user-get.md) if you need to fill in the `UF_HEAD` field
4. Create a department using [department.add](department-add.md) or update it using [department.update](department-update.md)
5. Delete a department using [department.delete](department-delete.md) if it is no longer needed

### If You Work with REST 3.0

1. Find the `id` of a department or team using [humanresources.node.list](./node/humanresources-node-list.md). If the `id` is already known, retrieve the data using [humanresources.node.get](./node/humanresources-node-get.md)
2. Create a new department or team using [humanresources.node.add](./node/humanresources-node-add.md), or update its properties using [humanresources.node.edit](./node/humanresources-node-edit.md)
3. Get user identifiers using [user.get](../user/user-get.md) if you need to prepare the participant list
4. Add participants using [humanresources.node.member.add](./node-member/humanresources-node-member-add.md) or set the full participant list using [humanresources.node.member.set](./node-member/humanresources-node-member-set.md)
5. If a department or team needs its own chats, channels, or collabs, configure them using [humanresources.node.communication.edit](./node-communication/humanresources-node-communication-edit.md)
6. If you need to find an employee or check which departments they belong to, use the methods of the [Employees](./employee/index.md) section

## Limitations and Recommendations

- Any user can read the structure through `department.*` methods. The `humanresources.*` methods take into account permissions to view departments and teams. Only a user with the permission to change the structure can modify it
- Permissions for departments and for teams are configured separately
- In `humanresources.node.*` methods, the `type` parameter distinguishes a department from a team: `DEPARTMENT` — department, `TEAM` — team
- Departments and teams use different sets of participant roles
- `department.*` methods work only with the `ID`, `NAME`, `SORT`, `PARENT`, and `UF_HEAD` fields
- Check the available fields before changing data: use the `*.field.list` and `*.field.get` methods for REST 3.0 or [department.fields](department-fields.md) for the previous API version

{% note tip "User documentation" %}

- [Company structure](https://helpdesk.bitrix24.com/open/18082492/)

{% endnote %}

## Connection with Other Objects

**Users.** In `department.*` methods, the department head is set by the `UF_HEAD` parameter — the numeric user identifier. The `humanresources.*` methods require user identifiers `userIds` to add, move, and remove participants. User identifiers are returned by [user.get](../user/user-get.md).

**Chats, channels, and collabs.** They can be created or bound right when a department or team is created using [humanresources.node.add](./node/humanresources-node-add.md). Later, connected chats, channels, and collabs can be retrieved using [humanresources.node.communication.list](./node-communication/humanresources-node-communication-list.md) and changed using [humanresources.node.communication.edit](./node-communication/humanresources-node-communication-edit.md).

**Employees.** To find an employee by name, use [humanresources.employee.search](./employee/humanresources-employee-search.md). The number of a user's subordinates by department is returned by [humanresources.employee.subordinates](./employee/humanresources-employee-subordinates.md), and employees from multiple departments by [humanresources.employee.multidepartment](./employee/humanresources-employee-multidepartment.md).

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
|| [department.delete](department-delete.md) | Deletes a department ||
|| [department.fields](department-fields.md) | Returns the department fields reference ||
|#

### REST 3.0 Methods

> Scope: [`humanresources`](../scopes/permissions.md)
>
> Who can execute the method: depends on the method

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
|| [humanresources.employee.subordinates](./employee/humanresources-employee-subordinates.md) | Returns the number of a user's subordinates by department ||
|| [humanresources.employee.count](./employee/humanresources-employee-count.md) | Returns the number of employees in the company structure ||
|| [humanresources.employee.multidepartment](./employee/humanresources-employee-multidepartment.md) | Returns employees who belong to multiple departments ||
|| [humanresources.employee.field.list](./employee/humanresources-employee-field-list.md) | Returns a list of employee fields ||
|| [humanresources.employee.field.get](./employee/humanresources-employee-field-get.md) | Returns the description of an employee field ||
|#
