# Participants of Groups and Projects: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The participant management methods let you retrieve the list of participants of a group or project, add and invite users, send requests to join, and change roles.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Frequently Asked Questions about Groups and Projects in Bitrix24](https://helpdesk.bitrix24.com/open/24633004/)

## Connection of Participants in Working Groups and Projects with Other Objects

**Users.** They are part of workgroups and projects. To retrieve a user ID, use the [user.get](./../../user/user-get.md) method.

**Groups and Projects.** To retrieve the ID of a group or project, use the [sonet_group.get](../sonet-group-get.md) method. You can retrieve the list of group participants using [sonet_group.user.get](./sonet-group-user-get.md). To retrieve the groups and projects of the current user, use [sonet_group.user.groups](../sonet-group-user-groups.md).

**Group Participant Roles.** A participant of a group or project can be the owner, a moderator, or a participant. To change a participant's role, use [sonet_group.user.update](./sonet-group-user-update.md).

Role codes in the `ROLE` field of the [sonet_group.user.get](./sonet-group-user-get.md) response:

#|
|| **Code** | **Role** ||
|| `A` | Owner ||
|| `E` | Moderator ||
|| `K` | Participant ||
|#

The `sonet_group.user.update` method accepts only `E` and `K` in the `ROLE` parameter. To change the owner, use [sonet_group.setowner](../sonet-group-setowner.md).

**Tasks.** Group participants can be assigned as task assignees and observers. Use the [tasks.task.add](../../tasks/tasks-task-add.md) and [tasks.task.update](../../tasks/tasks-task-update.md) methods.

## How to Get Started

1. Retrieve the user ID using the [user.get](./../../user/user-get.md) method.
2. Retrieve the group or project ID using the [sonet_group.get](../sonet-group-get.md) or [socialnetwork.api.workgroup.list](../socialnetwork-api-workgroup-list.md) method.
3. To add a user without an invitation, use [sonet_group.user.add](./sonet-group-user-add.md). To send an invitation, use [sonet_group.user.invite](./sonet-group-user-invite.md).
4. If the current user wants to join the group on their own, send a request using the [sonet_group.user.request](./sonet-group-user-request.md) method.
5. Check the participant list using the [sonet_group.user.get](./sonet-group-user-get.md) method. An invited user appears in the list after accepting the invitation, and a user who requested to join a closed group appears after the request is approved.
6. If needed, change the participant's role using the [sonet_group.user.update](./sonet-group-user-update.md) method.

## Overview of Methods {#all-methods}

> Scope: [`sonet_group`](./../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method and the user's permissions. The requirements are listed on the method pages

#|
|| **Method** | **Description** ||
|| [sonet_group.user.add](./sonet-group-user-add.md) | Adds a participant to the working group ||
|| [sonet_group.user.update](./sonet-group-user-update.md) | Changes a participant's role in the group ||
|| [sonet_group.user.get](./sonet-group-user-get.md) | Retrieves the list of participants in the group ||
|| [sonet_group.user.delete](./sonet-group-user-delete.md) | Removes a participant from the group ||
|| [sonet_group.user.invite](./sonet-group-user-invite.md) | Invites a participant to the group ||
|| [sonet_group.user.request](./sonet-group-user-request.md) | Sends a request from the current user to join the group ||
|#
