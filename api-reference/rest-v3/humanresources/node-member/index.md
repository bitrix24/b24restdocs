# Participants of Departments and Teams: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Participants of departments and teams define who belongs to a department or team and what role they occupy within the company structure. The methods `humanresources.node.member.*` assist in adding users, assigning roles, transferring participants between departments and teams, and removing them.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Company structure: New interface and features](https://helpdesk.bitrix24.com/open/23583540/)

## Working with Participants

1. Identify the required department or team and obtain the `nodeId` using the [humanresources.node.list](../node/humanresources-node-list.md) method. If the identifier is already known, verify the department or team data using the [humanresources.node.get](../node/humanresources-node-get.md) method.
2. Retrieve user identifiers using the [user.get](../../../user/user-get.md) method if you need to prepare `userIds` for adding, transferring, or removing participants.
3. Add users to the desired department or team using the [humanresources.node.member.add](./humanresources-node-member-add.md) method.
4. Use [humanresources.node.member.set](./humanresources-node-member-set.md) if you need to specify the complete composition of participants by roles in a single request.
5. Transfer participants to another department or team using the [humanresources.node.member.move](./humanresources-node-member-move.md) method if you need to change their position in the structure.
6. Remove participants using the [humanresources.node.member.remove](./humanresources-node-member-remove.md) method if they should no longer belong to the department or team.

## Section Limitations

- Access to view and modify participants depends on the permissions of the current user.
- The [humanresources.node.member.add](./humanresources-node-member-add.md) method assigns a single role for all users from `userIds`.

{% note tip "User Documentation" %}

- [Access permissions to the company structure](https://helpdesk.bitrix24.com/open/23580546/)

{% endnote %}

## Connection with Other Objects

**Departments and Teams.** All methods in this section use `nodeId` — the identifier of the department or team for which the composition of participants is being changed. The `nodeId` is obtained using the [humanresources.node.list](../node/humanresources-node-list.md) method. The department or team data can be verified using the [humanresources.node.get](../node/humanresources-node-get.md) method. To work with the hierarchy of departments and teams, use the methods in the [humanresources.node.*](../node/index.md) section.

**Users.** To add, transfer, and remove participants, `userIds` — user identifiers — are used. These can be obtained using the [user.get](../../../user/user-get.md) method. In the [humanresources.node.member.add](./humanresources-node-member-add.md) method, `userIds` are passed as a single list, while in [humanresources.node.member.set](./humanresources-node-member-set.md), they are provided as an object with lists by roles.

**Participant Roles.** A role defines a user's position in a department or team: leader, deputy, or employee. Role codes are passed in the `role` parameter of the [humanresources.node.member.add](./humanresources-node-member-add.md) method and in the keys of the `userIds` object in the [humanresources.node.member.set](./humanresources-node-member-set.md) method.

## Overview of Methods {#all-methods}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#| 
|| **Method** | **Description** ||
|| [humanresources.node.member.add](./humanresources-node-member-add.md) | Adds users to a department or team ||
|| [humanresources.node.member.set](./humanresources-node-member-set.md) | Updates the composition of participants in a department or team by roles ||
|| [humanresources.node.member.move](./humanresources-node-member-move.md) | Transfers users to another department or team ||
|| [humanresources.node.member.remove](./humanresources-node-member-remove.md) | Removes users from a department or team ||
|#

## Continue Your Exploration

- [{#T}](../node/index.md)
- [{#T}](../index.md)