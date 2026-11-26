# Participants of Departments and Teams: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Participants of departments and teams define who belongs to a department or team and what role they occupy within the company structure. The methods `humanresources.node.member.*` assist in adding users, assigning roles, transferring participants between departments and teams, and removing them.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Company structure: New interface and features](https://helpdesk.bitrix24.com/open/23583540/)

{% note info "" %}

The methods in this section belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

## Working With Participants

1. Retrieve the `nodeId` of the department or team using the [humanresources.node.list](../node/humanresources-node-list.md) method
2. Retrieve user identifiers using the [user.get](../../user/user-get.md) method
3. Choose the method for your task:
   - add employees or change their role — [humanresources.node.member.add](./humanresources-node-member-add.md). Other participants remain unchanged, and the employees stay in their current departments
   - set the full composition by roles — [humanresources.node.member.set](./humanresources-node-member-set.md). Participants not included in the request are removed from the department or team
   - transfer employees to another place — [humanresources.node.member.move](./humanresources-node-member-move.md). The employees are removed from all their departments and remain only in the target one
   - remove users — [humanresources.node.member.remove](./humanresources-node-member-remove.md). Other participants remain unchanged
4. Verify the composition of the department or team using the [humanresources.node.get](../node/humanresources-node-get.md) method: pass `members` in `select`. The `add`, `set`, and `move` methods skip unsuitable identifiers without an error, so `success: true` does not guarantee that all users were added


## Section Limitations

- The `add`, `set`, and `move` methods work only with employees — users who already belong to at least one department. Other identifiers are skipped without an error, and the method still returns `success: true`
- An employee cannot be removed from their only department: the `set` and `remove` methods return an error or a reason for the refusal
- The [humanresources.node.member.add](./humanresources-node-member-add.md) method assigns a single role for all users from `userIds`.
- Permissions depend on the object type: a department requires the "Add employees to department" and "Remove employees from department" permissions, and a team requires "Add team members" and "Remove team members". The field methods are available to any user

{% note tip "User Documentation" %}

- [Access permissions to the company structure](https://helpdesk.bitrix24.com/open/23580546/)

{% endnote %}


## Connection with Other Objects

**Departments and Teams.** All methods in this section use `nodeId` — the identifier of the department or team for which the composition of participants is being changed. The `nodeId` is obtained using the [humanresources.node.list](../node/humanresources-node-list.md) method. The department or team data can be verified using the [humanresources.node.get](../node/humanresources-node-get.md) method. To work with the hierarchy of departments and teams, use the methods in the [humanresources.node.*](../node/index.md) section.

**Users.** To add, transfer, and remove participants, `userIds` — user identifiers — are used. These can be obtained using the [user.get](../../user/user-get.md) method. In the [humanresources.node.member.add](./humanresources-node-member-add.md) method, `userIds` are passed as a single list, while in [humanresources.node.member.set](./humanresources-node-member-set.md), they are provided as an object with lists by roles.

**Participant Roles.** A role defines a user's position in a department or team: leader, deputy, or employee. Role codes are passed in the `role` parameter of the [humanresources.node.member.add](./humanresources-node-member-add.md) and [humanresources.node.member.move](./humanresources-node-member-move.md) methods and in the keys of the `userIds` object in the [humanresources.node.member.set](./humanresources-node-member-set.md) method. The set of roles depends on the object type: a team role for a department or a department role for a team returns the `INVALID_ROLE` error.

### Role Codes {#roles}

#|
|| **Code** | **Object Type** | **Role** ||
|| `MEMBER_HEAD` | Department | Department head ||
|| `MEMBER_DEPUTY_HEAD` | Department | Deputy department head ||
|| `MEMBER_EMPLOYEE` | Department | Employee ||
|| `MEMBER_TEAM_HEAD` | Team | Team leader ||
|| `MEMBER_TEAM_DEPUTY_HEAD` | Team | Deputy team leader ||
|| `MEMBER_TEAM_EMPLOYEE` | Team | Team member ||
|#

Example of `role` in the `add` method: `"role": "MEMBER_EMPLOYEE"`. Example of `userIds` in the `set` method: `{"MEMBER_HEAD": [7], "MEMBER_EMPLOYEE": [18, 25]}`.


## Overview of Methods {#all-methods}

> Scope: [`humanresources`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#| 
|| **Method** | **Description** ||
|| [humanresources.node.member.add](./humanresources-node-member-add.md) | Adds users to a department or team ||
|| [humanresources.node.member.set](./humanresources-node-member-set.md) | Updates the composition of participants in a department or team by roles ||
|| [humanresources.node.member.move](./humanresources-node-member-move.md) | Transfers users to another department or team ||
|| [humanresources.node.member.remove](./humanresources-node-member-remove.md) | Removes users from a department or team ||
|| [humanresources.node.member.field.list](./humanresources-node-member-field-list.md) | Returns a list of fields for a department or team member ||
|| [humanresources.node.member.field.get](./humanresources-node-member-field-get.md) | Returns the description of a department or team member field ||
|#


## Continue Your Exploration

- [{#T}](../node/index.md)
- [{#T}](../index.md)