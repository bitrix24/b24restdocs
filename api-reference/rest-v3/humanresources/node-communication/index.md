# Department and Team Communications: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Department and team communications connect the company's structural elements with chats, channels, and collabs. The methods `humanresources.node.communication.*` help retrieve current connections, bind existing communications, unbind them, or create default communications.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Add a department to a chat, channel, or collab in Bitrix24](https://helpdesk.bitrix24.com/open/25442614/)

## Getting Started

1. Obtain the `id` of the department or team using the [humanresources.node.list](../node/humanresources-node-list.md) method.
2. Check related chats, channels, and collabs using the [humanresources.node.communication.list](./humanresources-node-communication-list.md) method.
3. Modify connections using the [humanresources.node.communication.edit](./humanresources-node-communication-edit.md) method: pass the communication type, identifiers for binding, or a list for removal.

## Section Limitations

- Access to view and modify communications depends on the current user's permissions for departments and teams.

## Types of Communications

#|
|| **Type** | **Description** ||
|| `CHAT` | Chat associated with a department or team ||
|| `CHANNEL` | Channel associated with a department or team ||
|| `COLLAB` | Collab associated with a department or team ||
|#

## Connection with Other Objects

**Departments and Teams.** All methods use the identifier of the department or team. It can be obtained using the [humanresources.node.list](../node/humanresources-node-list.md) method or checked using the [humanresources.node.get](../node/humanresources-node-get.md) method.

**Chats and Channels.** The [humanresources.node.communication.edit](./humanresources-node-communication-edit.md) method accepts identifiers of existing chats or channels in `ids` and `removeIds`. Identifiers can be obtained using the [im.recent.list](../../../chats/im-recent-list.md) method.

**Collabs.** To bind or unbind a collab, pass `communicationType` with the value `COLLAB`. Collab identifiers can be obtained using the [socialnetwork.api.workgroup.list](../../../sonet-group/socialnetwork-api-workgroup-list.md) method.

## Overview of Methods {#all-methods}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [humanresources.node.communication.list](./humanresources-node-communication-list.md) | Returns related chats, channels, and collabs of a department or team ||
|| [humanresources.node.communication.edit](./humanresources-node-communication-edit.md) | Modifies related chats, channels, or collabs of a department or team ||
|#

## Continue Exploring

- [{#T}](../node/index.md)
- [{#T}](../node-member/index.md)
- [{#T}](../index.md)
