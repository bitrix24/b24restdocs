# Workgroups and Projects: Overview of Methods

Workgroups and projects in Bitrix24 help organize teamwork. In groups, you can:

- distribute tasks among participants, set deadlines, and track progress,

- exchange documents, store, and collaboratively edit files,

- discuss tasks in chats, leave comments, and hold online meetings.

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Workgroups and Projects in Bitrix24](https://helpdesk.bitrix24.com/open/24633004/)

## What distinguishes a group from a project

A project is a group with enhanced capabilities. Its main distinction from a group is the ability to set deadlines. The algorithm for creating a group and a project is identical: in both cases, use the method [sonet_group.create](./sonet-group-create.md). For a project, specify additional parameters:

- `PROJECT` — indicates that the created object is a project,

- `PROJECT_DATE_START` — the start date of the project,

- `PROJECT_DATE_FINISH` — the end date of the project.

## Connection of Workgroups and Projects with Other Objects

**Users**. Collaborate on tasks within workgroups and projects. Use the methods group sonet_group.user.* to manage workgroup participants: add, remove, assign roles, and permissions.

**Tasks**. Needed to distribute responsibilities among workgroup participants, track completion, and control deadlines. Create and modify tasks using the group of methods [tasks.task.*](../tasks/index.md).

**Drive**. A storage linked to a specific group or project containing necessary materials for work. To manage storages, use the group of methods [disk.storage.*](../disk/storage/index.md).

**Universal Lists**. Structured lists of items within workgroups. Needed to create registries or data storages, sort and filter information, and automate accounting. Create, update, and delete universal lists using the methods group [lists.lists.*](../lists/lists/index.md).

**Feed**. Use the method [log.blogpost.add](../log/log-blogpost-add.md) to publish messages that will only be visible to users added to the group.

> User documentation
>
> - [How to create a group and project](https://helpdesk.bitrix24.com/open/22796428/)

## Widgets for Workgroups and Projects

Add your items to dropdown menus to enhance the functionality of workgroups and projects:

- [Main dropdown menu item for the project SONET_GROUP_DETAIL_TAB](../widgets/workgroups/index.md).

- [Dropdown menu item above the task list TASK_GROUP_LIST_TOOLBAR](../widgets/workgroups/toolbar.md).

- [Main dropdown menu item near the Automation rules settings TASK_ROBOT_DESIGNER_TOOLBAR](../widgets/workgroups/robot-designer-toolbar.md).

Specify the code for the specific widget placement in the `PLACEMENT` parameter of the method [placement.bind](../widgets/placement-bind.md).

## Specialized Workgroups: Scrum and Flow

**Scrum in Bitrix24**. A tool for organizing teamwork using the Scrum methodology. Allows breaking projects into sprints — short iterations during which the team completes a specific volume of tasks.

**Flow in Bitrix24**. A tool for organizing teamwork. Allows gathering tasks in one place and quickly distributing them among assignees.

> User documentation
> 
> - [Bitrix24 Flows: Getting Started](https://helpdesk.bitrix24.com/open/21415178/)
> 
> - [Bitrix24 Scrum](https://helpdesk.bitrix24.com/open/14786248/)

Scrum and Flow are implemented based on workgroups.

To create a Scrum, use the method [creating a new group](./sonet-group-create.md). To make the group a Scrum, fill in the `SCRUM_MASTER_ID` field.

To link Flow to a group, use the identifier `groupId`. To obtain the identifier, use the method [creating a new group](./sonet-group-create.md) or the method [getting the list of groups](./socialnetwork-api-workgroup-list.md).

## Overview of Methods and Events {#all-methods}

> Scope: [`sonet`](../scopes/permissions.md)
>
> Who can execute the method: any user

{% list tabs %}

- Methods

    #|
    || **Method**                        | **Description**                                ||
    || [sonet_group.create](./sonet-group-create.md)              | Creates a group                              ||
    || [sonet_group.update](./sonet-group-update.md)               | Modifies group parameters                     ||
    || [socialnetwork.api.workgroup.get](./socialnetwork-api-workgroup-get.md)  | Retrieves data on the workgroup              ||
    || [socialnetwork.api.workgroup.list](./socialnetwork-api-workgroup-list.md) | Retrieves a list of workgroups                ||
    || [sonet_group.get](./sonet-group-get.md)                  | Retrieves a list of groups                    ||
    || [sonet_group.feature.access](./sonet-group-feature-access.md)       | Checks the rights of the current user        ||
    || [sonet_group.user.groups](./sonet-group-user-groups.md)          | Retrieves a list of groups for the current user ||
    || [sonet_group.setowner](./sonet-group-setowner.md)             | Changes the owner of the group                ||
    || [sonet_group.delete](./sonet-group-delete.md)               | Deletes a group                               ||
    |#

    ### Managing Users in Groups
    #|
    || **Method**                        | **Description**                                ||
    || [sonet_group.user.invite](./members/sonet-group-user-invite.md)          | Invites users to the group                    ||
    || [sonet_group.user.request](./members/sonet-group-user-request.md)         | Sends a request to join the group             ||
    || [sonet_group.user.add](./members/sonet-group-user-add.md)             | Adds users to the group                       ||
    || [sonet_group.user.update](./members/sonet-group-user-update.md)          | Changes the user's role in the group          ||
    || [sonet_group.user.get](./members/sonet-group-user-get.md)             | Retrieves a list of group members              ||
    || [sonet_group.user.delete](./members/sonet-group-user-delete.md)          | Removes users from the group                   ||
    |#

- Events

    #|
    || **Event**                      | **Triggered**                              ||
    || [onSonetGroupAdd](./events/on-sonet-group-add.md)       | After adding a new workgroup               ||
    || [onSonetGroupUpdate](./events/on-sonet-group-update.md) | After modifying a workgroup                ||
    || [onSonetGroupDelete](./events/on-sonet-group-delete.md) | When deleting a workgroup                  ||
    |#

{% endlist %}