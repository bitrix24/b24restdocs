# Departments and Teams: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Departments and teams in Bitrix24 form the structure of the company. The methods `humanresources.node.*` help retrieve data about a department or team, find the required department or team, build a hierarchy, create a new department or team, and change their position in the tree.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Company structure: New interface and features](https://helpdesk.bitrix24.com/open/23583540/)

## Differences Between Departments and Teams

**Departments.** Use departments when you need to describe a permanent structure of the company: areas of work, subordination, and staff composition. When creating a department, you can specify the parent department, name, description, managers, deputies, and subordinates.

**Teams.** Use teams when you need to gather employees from different departments for a common task or project. A team also has a leader and a deputy. You can set a description, color, and parent department or team for the team.

## Working with Departments and Teams

1. Retrieve field descriptions using the methods [humanresources.node.field.list](./humanresources-node-field-list.md) and [humanresources.node.field.get](./humanresources-node-field-get.md) if you need to understand in advance what values can be passed for a department or team.
2. Get a list of departments or teams through [humanresources.node.list](./humanresources-node-list.md). In the request, specify `type`: `DEPARTMENT` for departments or `TEAM` for teams.
3. Select the required `id` and request details through [humanresources.node.get](./humanresources-node-get.md). If you need to find a department or team by part of the name, use [humanresources.node.search](./humanresources-node-search.md).
4. If working with a hierarchy, obtain the `parentId` of the parent department or team through [humanresources.node.list](./humanresources-node-list.md) or [humanresources.node.get](./humanresources-node-get.md), and then request child departments and teams through [humanresources.node.children](./humanresources-node-children.md).
5. Create a new department or team using the method [humanresources.node.add](./humanresources-node-add.md). When creating, you can immediately pass a list of future participants, as well as create or link chats, channels, and collabs.
6. Modify the properties of a department or team using the method [humanresources.node.edit](./humanresources-node-edit.md) or move them to a new parent using the method [humanresources.node.move](./humanresources-node-move.md).
7. Use [humanresources.node.count](./humanresources-node-count.md) when you need to quickly find out the number of departments and teams without loading the full list.

## Section Limitations

- Access to view and modify departments and teams depends on the permissions of the current user.
- Chats, channels, and collabs can only be created or linked during the creation of a department or team.

{% note tip "User Documentation" %}

- [Create and change departments in the company structure](https://helpdesk.bitrix24.com/open/23599408/)
- [Cross-functional teams in Bitrix24](https://helpdesk.bitrix24.com/open/25609471/)
- [Add department members to chats, channels, and collabs](https://helpdesk.bitrix24.com/open/25442614/)
- [Access permissions to the company structure](https://helpdesk.bitrix24.com/open/23580546/)

{% endnote %}

## Connection with Other Objects

**Users.** When creating a department or team, the method [humanresources.node.add](./humanresources-node-add.md) accepts `userIds` with the identifiers of managers, deputies, and employees. User identifiers can be obtained using the method [user.get](../../../user/user-get.md). For departments, the parameter `moveUsersToNode` determines whether employees will be transferred to the new department or added as participants.

**Chats and Channels.** When creating a department or team, you can immediately create a separate chat or channel through `createChat` and `createChannel`, or link existing chats and channels through `bindingChatIds` and `bindingChannelIds`. Identifiers of existing chats and channels can be obtained using the method [im.recent.list](../../../chats/im-recent-list.md).

**Collabs.** When creating a department or team, you can create a new collab through `createCollab` or link existing collabs through `bindingCollabIds`, if collabs are available in the plan. Identifiers of collabs can be obtained using the method [socialnetwork.api.workgroup.list](../../../sonet-group/socialnetwork-api-workgroup-list.md).

## Overview of Methods {#all-methods}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can perform the method: depends on the method

#| 
|| **Method** | **Description** ||
|| [humanresources.node.add](./humanresources-node-add.md) | Creates a department or team ||
|| [humanresources.node.edit](./humanresources-node-edit.md) | Updates fields of a department or team ||
|| [humanresources.node.get](./humanresources-node-get.md) | Returns a department or team by identifier ||
|| [humanresources.node.list](./humanresources-node-list.md) | Returns a list of departments and teams ||
|| [humanresources.node.search](./humanresources-node-search.md) | Searches for departments and teams by name ||
|| [humanresources.node.children](./humanresources-node-children.md) | Returns child departments and teams ||
|| [humanresources.node.count](./humanresources-node-count.md) | Returns the number of departments and teams ||
|| [humanresources.node.move](./humanresources-node-move.md) | Moves a department or team to a new parent ||
|| [humanresources.node.field.list](./humanresources-node-field-list.md) | Returns a list of fields for a department or team ||
|| [humanresources.node.field.get](./humanresources-node-field-get.md) | Returns the description of a field for a department or team ||
|#

## Continue Learning

- [{#T}](../node-member/index.md)
- [{#T}](../index.md)