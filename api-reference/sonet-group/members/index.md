# Participants of Groups and Projects: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Participants of groups and projects are users who are part of working groups and projects. They can have different roles and permissions.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Frequently Asked Questions about Groups and Projects in Bitrix24](https://helpdesk.bitrix24.com/open/24633004/)

## Connection of Participants in Working Groups and Projects with Other Objects

**Users.** They are part of workgroups and projects. To retrieve a user ID, use the [user.get](./../../user/user-get.md) method.

**Groups and Projects.** To retrieve the ID of a group or project, use the [sonet_group.get](../sonet-group-get.md) method. You can retrieve the list of group participants using [sonet_group.user.get](./sonet-group-user-get.md).

**Group Participant Roles.** Participants in workgroups and projects can have different roles: administrator, moderator, participant. Access permissions depend on these roles. To change a participant's role, use the [sonet_group.user.update](./sonet-group-user-update.md) method.

**Tasks.** Participants of groups and projects can be assigned as task assignees. When creating a task, add participants using the [tasks.task.add](./../../tasks/tasks-task-add.md) method. Specify the parameters `CREATED_BY` — task creator, `RESPONSIBLE_ID` — primary assignee, `ACCOMPLICES` — participants, `AUDITORS` — observers. When updating a task, you can add participants using the [tasks.task.update](./../../tasks/tasks-task-update.md) method. To do this, pass the updated `RESPONSIBLE_ID`, `ACCOMPLICES`, and `AUDITORS` parameters.

## How to Get Started

1. Retrieve the user ID using the [user.get](./../../user/user-get.md) method.
2. Retrieve the group or project ID using the [sonet_group.get](../sonet-group-get.md) or [socialnetwork.api.workgroup.list](../socialnetwork-api-workgroup-list.md) method.
3. Add a participant using the [sonet_group.user.add](./sonet-group-user-add.md) method or send an invitation using the [sonet_group.user.invite](./sonet-group-user-invite.md) method.
4. Check the participant list using the [sonet_group.user.get](./sonet-group-user-get.md) method.
5. If needed, change the participant's role using the [sonet_group.user.update](./sonet-group-user-update.md) method.

## Overview of Methods {#all-methods}

> Scope: [`sonet`](./../../scopes/permissions.md)
>
> Who can execute the methods: any user

#|
|| **Method** | **Description** ||
|| [sonet_group.user.add](./sonet-group-user-add.md)  | Adds a participant to the working group ||
|| [sonet_group.user.invite](./sonet-group-user-invite.md) | Invites a participant to the group ||
|| [sonet_group.user.request](./sonet-group-user-request.md) | Sends a request to join the group ||
|| [sonet_group.user.delete](./sonet-group-user-delete.md) | Removes a participant from the group ||
|| [sonet_group.user.get](./sonet-group-user-get.md) | Retrieves the list of participants in the group ||
|| [sonet_group.user.update](./sonet-group-user-update.md) | Changes a participant's role in the group || 
|#
