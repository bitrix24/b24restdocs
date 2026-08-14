# Workgroups and Projects: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Workgroups and projects in Bitrix24 help organize team collaboration. In groups, you can:

- distribute tasks among participants, set deadlines, and track progress

- exchange documents, store, and collaboratively edit files

- discuss tasks in chats, leave comments, and hold online meetings

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Workgroups and Projects in Bitrix24](https://helpdesk.bitrix24.com/open/24633004/)

## What Distinguishes a Group from a Project

A project is a group with enhanced capabilities. Its main distinction from a group is the ability to set deadlines. The algorithm for creating a group and a project is identical: in both cases, use the method [sonet_group.create](./sonet-group-create.md). For a project, specify additional parameters:

- `PROJECT` — indicates that the created object is a project

- `PROJECT_DATE_START` — the project's start date

- `PROJECT_DATE_FINISH` — the project's end date

## Connection of Workgroups and Projects with Other Objects

**Users.** Collaborate on tasks within workgroups and projects. To manage workgroup participants, use the [sonet_group.user.*](./members/index.md) method group: add and remove users, assign roles and permissions.

**Tasks.** Needed to distribute responsibilities among workgroup participants, track completion, and control deadlines. Create and update tasks using the [tasks.task.*](../tasks/index.md) method group.

**Drive.** A storage linked to a specific group or project containing materials needed for work. To manage storages, use the [disk.storage.*](../disk/storage/index.md) method group.

**Universal Lists.** Structured lists of items within workgroups. Needed to create registries or data storages, sort and filter information, and automate accounting. Create, update, and delete universal lists using the [lists.lists.*](../lists/lists/index.md) method group.

**News Feed.** Publish messages for group participants using the [log.blogpost.add](../log/log-blogpost-add.md) method.

{% note tip "User Documentation" %}

- [How to Create a Group and Project](https://helpdesk.bitrix24.com/open/22796428/)

{% endnote %}

## How to Get Started

1. Create a group or project using the [sonet_group.create](./sonet-group-create.md) method.
2. Retrieve the group identifier using the [socialnetwork.api.workgroup.list](./socialnetwork-api-workgroup-list.md) or [sonet_group.get](./sonet-group-get.md) method.
3. Add participants using the [sonet_group.user.*](./members/index.md) methods.
4. Connect related tools: tasks, Drive, universal lists, or News Feed messages.
5. To track group creation, update, and deletion, subscribe to [events](./events/index.md).

## Widgets for Workgroups and Projects

Add your items to menus to enhance the functionality of workgroups and projects:

- [Workgroup menu item SONET_GROUP_DETAIL_TAB](../widgets/workgroups/detail-tab.md)

- [Workgroup extensions menu item SONET_GROUP_TOOLBAR](../widgets/workgroups/toolbar.md)

- [Button in the workgroup automation rules designer SONET_GROUP_ROBOT_DESIGNER_TOOLBAR](../widgets/workgroups/robot-designer-toolbar.md)

Specify the widget embedding code in the `PLACEMENT` parameter of the method [placement.bind](../widgets/placement-bind.md). How to choose a placement and what the handler receives is described in the [overview of placements](../widgets/workgroups/index.md).

## Specialized Workgroups: Scrum and Flow

**Scrum in Bitrix24**. A tool for organizing team collaboration using the Scrum methodology. It allows breaking projects into sprints — short iterations during which the team completes a specific volume of tasks.

**Flow in Bitrix24**. A tool for organizing team collaboration. It allows gathering tasks in one place and quickly distributing them among performers.

{% note tip "User Documentation" %}

- [Bitrix24 Flows: Getting Started](https://helpdesk.bitrix24.com/open/21415178/)
- [How to Work in Scrum](https://helpdesk.bitrix24.com/open/21300770/)

{% endnote %}

Scrum and Flow are implemented based on workgroups.

To create a Scrum, use the method [creating a new group](./sonet-group-create.md). To make a group a Scrum, fill in the `SCRUM_MASTER_ID` field.

To link a Flow to a group, use the identifier `groupId`. To obtain the identifier, use the method [creating a new group](./sonet-group-create.md) or the method [getting a list of groups](./socialnetwork-api-workgroup-list.md).

## Overview of Methods and Events {#all-methods}

> Scope: [`sonet`](../scopes/permissions.md)
>
> Who can execute the methods: any user

{% list tabs %}

- Methods

    #| 
    || **Method**                        | **Description**                                ||
    || [sonet_group.create](./sonet-group-create.md)              | Creates a group                              ||
    || [sonet_group.update](./sonet-group-update.md)               | Modifies group parameters                     ||
    || [socialnetwork.api.workgroup.get](./socialnetwork-api-workgroup-get.md)  | Retrieves data on the workgroup              ||
    || [socialnetwork.api.workgroup.list](./socialnetwork-api-workgroup-list.md) | Retrieves a list of workgroups                ||
    || [sonet_group.get](./sonet-group-get.md)                  | Retrieves a list of groups                    ||
    || [sonet_group.feature.access](./sonet-group-feature-access.md)       | Checks the current user's permissions         ||
    || [sonet_group.user.groups](./sonet-group-user-groups.md)          | Retrieves a list of the current user's groups ||
    || [sonet_group.setowner](./sonet-group-setowner.md)             | Changes the group owner                       ||
    || [sonet_group.delete](./sonet-group-delete.md)               | Deletes a group                               ||
    |#

    ### Managing Users in Groups
    #| 
    || **Method**                        | **Description**                                ||
    || [sonet_group.user.invite](./members/sonet-group-user-invite.md)          | Invites users to the group                    ||
    || [sonet_group.user.request](./members/sonet-group-user-request.md)         | Sends a request to join the group             ||
    || [sonet_group.user.add](./members/sonet-group-user-add.md)             | Adds users to the group                       ||
    || [sonet_group.user.update](./members/sonet-group-user-update.md)          | Changes a user's role in the group            ||
    || [sonet_group.user.get](./members/sonet-group-user-get.md)             | Retrieves a list of group participants         ||
    || [sonet_group.user.delete](./members/sonet-group-user-delete.md)          | Removes users from the group                  ||
    |#

- Events

    #| 
    || **Event**                      | **Triggered**                              ||
    || [onSonetGroupAdd](./events/on-sonet-group-add.md)       | After adding a new workgroup               ||
    || [onSonetGroupUpdate](./events/on-sonet-group-update.md) | After modifying a workgroup                ||
    || [onSonetGroupDelete](./events/on-sonet-group-delete.md) | At the moment of deleting a workgroup       ||
    |#

{% endlist %}
