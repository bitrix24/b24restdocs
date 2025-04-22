# News Feed: Overview of Methods

The news feed of the corporate account resembles feeds in social networks. It allows employees to stay updated on company events. It displays news, messages, tasks, polls, congratulations, and much more.

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [News Feed](https://helpdesk.bitrix24.com/section/47478/)

## Connection of the Feed with Other Objects

**Users**. To send a message in the news feed to specific users, you need to know their IDs. You can obtain the list of users using the [user.get](./../user/user-get.md) method.

**Workgroups and Projects**. Messages in the news feed can be directed to participants of workgroups and projects using the IDs of the workgroups and projects. The list of groups is available through the [sonet_group.get](./../sonet-group/sonet-group-get.md) method.

**Company Departments**. To address a message in the feed to company departments, you need to know the IDs of the departments. You can get the list of departments using the [department.get](./../departments/department-get.md) method.

**Files**. You can attach files from local storage, Bitrix24 Drive, or the cloud to messages in the news feed. To attach a file to a message, pass an array containing the file name and a string with `Base64` in the `Files` field. To get information about the attached file, use the [disk.attachedObject.get](./../disk/attached-object/disk-attached-object-get.md) method.

{% note tip "Typical use-cases and scenarios" %}

- [How to upload files](../files/how-to-upload-files.md)

{% endnote %}

**Calendar Events**. These are automatically displayed in the news feed. You can create and edit calendar events using methods from the [calendar.events.*](./../calendar/events/index.md) group.

**Tasks**. These are automatically displayed in the news feed. To create and edit tasks, use methods from the [tasks.*](./../tasks/index.md) group.

**New Employees**. Users can be created using the [user.add](./../user/user-add.md) method. The news feed automatically publishes messages about newly hired employees if permitted by the settings.

{% note tip "User documentation" %}

- [Bitrix24 Settings](https://helpdesk.bitrix24.com/open/19093214/)

{% endnote %}

## Starting Workflows from the Feed

Workflows can be initiated directly from the news feed without navigating to other sections.

{% note tip "User documentation" %}

- [Workflows in the News Feed](https://helpdesk.bitrix24.com/open/9717699/)

{% endnote %}

Use the [bizproc.workflow.template.list](./../bizproc/template/bizproc-workflow-template-list.md) method to get a list of available workflows and their identifiers. To filter only the workflows related to the news feed, specify `MODULE_ID: "lists"` and `ENTITY: "BizprocDocument"`.

With the [bizproc.workflow.start](./../bizproc/bizproc-workflow-start.md) method, start a workflow by specifying its ID. To link the workflow to the news feed, associate it with a list item. To work with list items, use the methods from the [lists.element.*](./../lists/elements/index.md) group. To create or retrieve a list item associated with the news feed workflow, specify `IBLOCK_TYPE_ID: bitrix_processes`. If a workflow is started for a list item, the news feed will automatically create a message.

## **Managing Messages in the Feed**

**How to add, modify, or delete a message**. You can add, modify, and delete messages in the news feed using methods. Use the [log.blogpost.add](./../log/log-blogpost-add.md) method to add a message to the news feed on behalf of the current user. You can modify a message using the [log.blogpost.update](./../log/log-blogpost-update.md) method. To delete a message, apply the [log.blogpost.delete](./../log/log-blogpost-delete.md) method.

**How to manage comments**. Use the [log.blogcomment.add](./../log/log-blogcomment-add.md) method to add comments to messages and the [log.blogcomment.delete](./../log/log-blogcomment-add.md) method to delete comments. The [log.blogcomment.user.get](./../log/log-blogcomment-add.md) method returns comments on messages and can filter them by ID range (`FIRST_ID`, `LAST_ID`). If no filtering parameters are provided, the method will return all comments available to the current user, considering their access permissions.

**How to find out which users have read an important message**. A message published in the news feed can be marked as important. To view the list of users who have read an important message, use the [log.blogpost.getusers.important](./log-blogpost-getusers-important.md) method. The combination of "important message and tracking the list of readers" can be used for automated mass notification scenarios.

**How to get a list of messages**. The [log.blogpost.get](./log-blogpost-get.md) method allows you to access messages available to the user in the news feed.

**What events are available**. The events `OnLiveFeedPostAdd`, `OnLiveFeedPostUpdate`, and `OnLiveFeedPostDelete` allow you to track the appearance, modification, and deletion of messages.

## Overview of Methods and Events {#all-methods}

> Scope: [`log`](../scopes/permissions.md)
> 
> Who can execute the method: any user

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [log.blogcomment.add](./log-blogcomment-add.md) | Adds a comment to a news feed message ||
    || [log.blogpost.add](./log-blogpost-add.md) | Adds a message to the news feed on behalf of the current user ||
    || [log.blogpost.update](./log-blogpost-update.md) | Modifies a news feed message ||
    || [log.blogpost.get](./log-blogpost-get.md) | Retrieves messages available to the user in the news feed ||
    || [log.blogpost.getusers.important](./log-blogpost-getusers-important.md) | Views users who have read an important message ||
    || [log.blogpost.share](./log-blogpost-share.md) | Adds recipients to a news feed message ||
    || [log.blogpost.delete](./log-blogpost-delete.md) | Deletes a news feed message ||
    |#

- Events

    #| 
    || **Event** | **Triggered by** ||
    || [OnLiveFeedPostAdd](./events/on-live-feed-post-add.md) | When a message is added to the news feed ||
    || [OnLiveFeedPostDelete](./events/on-live-feed-post-delete.md) | When a message is deleted from the news feed ||
    || [OnLiveFeedPostUpdate](./events/on-live-feed-post-update.md) | When a message is edited in the news feed ||
    |#

{% endlist %}