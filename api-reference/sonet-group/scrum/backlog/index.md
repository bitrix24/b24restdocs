# Backlog in Scrum: Overview of Methods

The backlog is a list of all the team's tasks. Tasks, requests, ideas, and feedback are recorded and prioritized. Team members review the backlog and decide which tasks to take on in a [sprint](../sprint/index.md).

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [Bitrix24.Scrum](https://helpdesk.bitrix24.com/open/14786248/)

## Connection of the Backlog with Other Objects

**Workgroup.** The backlog is linked to a workgroup (scrum) by the group identifier `groupId`. You can obtain the identifier using the [create new group](../../sonet-group-create.md) method or the [get list of groups](../../socialnetwork-api-workgroup-list.md) method. A group is considered a scrum if the `SCRUM_MASTER_ID` field is filled.

**User.** The backlog is associated with users by their numerical identifier in the `createdBy` and `modifiedBy` parameters. You can obtain the user identifier using the [user.get](../../../user/user-get.md) method.

{% note tip "User Documentation" %}

- [How to create a group and project](https://helpdesk.bitrix24.com/open/22796428/)

{% endnote %}

## Features of the add and delete Methods

There can only be one backlog in Scrum; Bitrix24 creates it automatically.

The [add backlog to Scrum](./tasks-api-scrum-backlog-add.md) method is useful only when importing data from another system, when you need to create a backlog after creating the Scrum.

Use the [delete backlog](./tasks-api-scrum-backlog-delete.md) method if you mistakenly added a backlog to a group or project that is not a scrum. If you delete the backlog in Scrum, Bitrix24 will automatically recreate it when opening the planning page.

## Overview of Methods {#all-methods}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can perform the method: any user

#|
|| **Method** | **Description** ||
|| [tasks.api.scrum.backlog.add](./tasks-api-scrum-backlog-add.md) | Adds a backlog to Scrum ||
|| [tasks.api.scrum.backlog.update](./tasks-api-scrum-backlog-update.md) | Updates the backlog ||
|| [tasks.api.scrum.backlog.get](./tasks-api-scrum-backlog-get.md) | Retrieves field values of the backlog by the Scrum identifier ||
|| [tasks.api.scrum.backlog.delete](./tasks-api-scrum-backlog-delete.md) | Deletes the backlog ||
|| [tasks.api.scrum.backlog.getFields](./tasks-api-scrum-backlog-get-fields.md) | Retrieves available fields of the backlog ||
|#