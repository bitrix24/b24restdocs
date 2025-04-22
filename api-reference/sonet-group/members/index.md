# Participants of Groups and Projects: Overview of Methods

Participants of groups and projects are users who are part of working groups and projects. They can have different roles and permissions.

> Quick navigation: [all methods](#all-methods)
> 
> User documentation: [Frequently Asked Questions about Groups and Projects in Bitrix24](https://helpdesk.bitrix24.com/open/24633004/)

## Connection of Participants in Working Groups and Projects with Other Objects

**Users**. They are part of working groups and projects. To get the user ID, use the method [user.get](./../../user/user-get.md).

**Groups and Projects**. To get the ID of a group or project, use the method [sonet_group.get](../sonet-group-get.md). The list of group participants can be obtained through [sonet_group.user.get](./sonet-group-user-get.md).

**Roles of Group Participants**. Participants in working groups and projects can have different roles: administrator, moderator, participant. Access permissions depend on these roles. To change a participant's role, use the method [sonet_group.user.update](./sonet-group-user-update.md).

**Tasks**. Participants of groups and projects can be assigned as task performers. When creating a task, add participants using the method [tasks.task.add](./../../tasks/tasks-task-add.md). Specify the parameters `CREATED_BY` — task creator, `RESPONSIBLE_ID` — main performer, `ACCOMPLICES` — participants, `AUDITORS` — observers. When modifying a task, you can add participants through the method [tasks.task.update](./../../tasks/tasks-task-update.md). For this, pass the updated parameters `RESPONSIBLE_ID`, `ACCOMPLICES`, `AUDITORS`.

## Overview of Methods {#all-methods}

> Scope: [`sonet`](./../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [sonet_group.user.add](./sonet-group-user-add.md)  | Adds a participant to the working group ||
|| [sonet_group.user.invite](./sonet-group-user-invite.md) | Invites a participant to the group ||
|| [sonet_group.user.request](./sonet-group-user-request.md) | Sends a request to join the group ||
|| [sonet_group.user.delete](./sonet-group-user-delete.md) | Removes a participant from the group ||
|| [sonet_group.user.get](./sonet-group-user-get.md) | Retrieves the list of participants in the group ||
|| [sonet_group.user.update](./sonet-group-user-update.md) | Changes a participant's role in the group || 
|#