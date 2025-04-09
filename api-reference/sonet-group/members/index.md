# Group and Project Participants: Overview of Methods

Group and project participants are users who are part of workgroups and projects. They can have different roles and permissions.

> Quick navigation: [all methods](#all-methods)
> 
> User documentation: [Frequently Asked Questions about Groups and Projects in Bitrix24](https://helpdesk.bitrix24.com/open/24633004/)

## Connection of Workgroup and Project Participants with Other Objects

**Users**. They are part of workgroups and projects. To get the `ID` of a user, use the `user.get` method.

**Groups and Projects**. To get the `ID` of a group or project, use the `sonet_group.get` method. The list of group participants can be obtained through `sonet_group.user.get`.

**Roles of Group Participants**. Participants in workgroups and projects can have different roles: administrator, moderator, participant. Access permissions to functionality depend on these roles. To change a participant's role, use the `sonet_group.user.update` method.

**Tasks**. Participants in groups and projects can be assigned as task assignees. To add a participant to a task, use the `task.member.add` method. The method `task.member.list` allows you to get the list of task participants.

## Overview of Methods {#all-methods}

> Scope: [`sonet`](./../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [sonet_group.user.add](./sonet-group-user-add.md)  | Adds a participant to a workgroup ||
|| [sonet_group.user.invite](./sonet-group-user-invite.md) | Invites a participant to a group ||
|| [sonet_group.user.request](./sonet-group-user-request.md) | Sends a request to join a group ||
|| [sonet_group.user.delete](./sonet-group-user-delete.md) | Removes a participant from a group ||
|| [sonet_group.user.get](./sonet-group-user-get.md) | Retrieves the list of group participants ||
|| [sonet_group.user.update](./sonet-group-user-update.md) | Changes a participant's role in a group ||
|#