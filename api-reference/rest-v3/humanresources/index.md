# Company Structure in REST 3.0: Overview of Sections

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The company structure illustrates the departments that make up the organization and how they are interconnected. Each department and team can define the roles of participants: leader, deputy, and staff members. This helps maintain an up-to-date organizational chart, quickly locate the necessary department or employee, configure collaborative work, and utilize the structure in related Bitrix24 tools.

The methods in this section work with two groups of objects:

- [Departments and Teams](./node/index.md) — create departments and teams, retrieve their lists, data, child elements, and field descriptions.
- [Department and Team Members](./node-member/index.md) — add users, assign roles, transfer, and remove participants.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Company structure: New interface and features](https://helpdesk.bitrix24.com/open/23583540/)

## Getting Started

1. Retrieve the list of departments or teams via [humanresources.node.list](./node/humanresources-node-list.md) to identify the required `id`.
2. Check the data of a department or team through [humanresources.node.get](./node/humanresources-node-get.md) if the identifier is already known.
3. Create a new department or team via [humanresources.node.add](./node/humanresources-node-add.md) or modify their properties through [humanresources.node.edit](./node/humanresources-node-edit.md).
4. Obtain user identifiers using the [user.get](../../user/user-get.md) method if you need to prepare the list of participants.
5. Add participants via [humanresources.node.member.add](./node-member/humanresources-node-member-add.md) or set the complete composition through [humanresources.node.member.set](./node-member/humanresources-node-member-set.md).

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

**Users.** To add, transfer, and remove participants, user identifiers `userIds` are required. These can be obtained using the [user.get](../../user/user-get.md) method.

**Chats, Channels, and Collabs.** When creating a department or team, the [humanresources.node.add](./node/humanresources-node-add.md) method allows you to create or link related chats, channels, and collabs immediately.

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

## Continue Learning

- [{#T}](./node/index.md)
- [{#T}](./node-member/index.md)
- [{#T}](../index.md)