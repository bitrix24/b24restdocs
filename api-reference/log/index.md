# News Feed: Overview of Methods

The corporate news feed resembles social media feeds. It allows employees to stay updated on company events. It displays news, messages, tasks, polls, congratulations, and much more.

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [News Feed](https://helpdesk.bitrix24.com/section/47478/)

## Connection of the Feed with Other Objects

**Users**. To send a message in the News Feed to specific users, you need to know their IDs. You can obtain the list of users using the [user.get](./../user/user-get.md) method.

**Workgroups and Projects**. Messages in the News Feed can be directed to participants of workgroups and projects using the IDs of the workgroups and projects. The list of groups is available through the [sonet_group.get](./../sonet-group/sonet-group-get.md) method.

**Company Departments**. To address a message in the Feed to company departments, you need to know the department IDs. You can get the list of departments using the [department.get](./../departments/department-get.md) method.

**Files**. You can attach files from local storage, Bitrix24 Drive, or the cloud to messages in the News Feed. To attach a file to a message, pass an array containing the file name and a string with `Base64` in the `Files` field. To get information about the attached file, use the [disk.attachedObject.get](./../disk/attached-object/disk-attached-object-get.md) method.

{% note tip "Typical use-cases and scenarios" %}

- [How to upload files](../files/how-to-upload-files.md)

{% endnote %}

**Calendar Events**. These are automatically displayed in the News Feed. You can create and edit calendar events using methods from the [calendar.events.*](./../calendar/events/index.md) group.

**Tasks**. These are automatically displayed in the News Feed. To create and edit tasks, use methods from the [tasks.*](./../tasks/index.md) group.

**New Employees**. Users can be created using the [user.add](./../user/user-add.md) method. The News Feed automatically publishes messages about newly hired employees if permitted by the settings.

{% note tip "User documentation" %}

- [Bitrix24 Settings](https://helpdesk.bitrix24.com/open/19093214/)

{% endnote %}

## Starting Workflows from the Feed

Workflows can be initiated directly from the News Feed without navigating to other sections.

{% note tip "User documentation" %}

- [Workflows in the News Feed](https://helpdesk.bitrix24.com/open/9717699/)

{% endnote %}

Use the [bizproc.workflow.template.list](./../bizproc/template/bizproc-workflow-template-list.md) method to get a list of available workflows and their identifiers. To filter only the workflows related to the News Feed, specify `MODULE_ID: "lists"` and `ENTITY: "BizprocDocument"`.

With the [bizproc.workflow.start](./../bizproc/bizproc-workflow-start.md) method, start a workflow by specifying its ID. To link the workflow to the News Feed, associate it with a list item. To work with list items, use the methods from the [lists.element.*](./../lists/elements/index.md) group. To create or retrieve a list item associated with the News Feed workflow, specify `IBLOCK_TYPE_ID: bitrix_processes`. If the workflow is started for a list item, the News Feed will automatically create a message.

## **Managing Messages in the Feed**

**How to add, modify, or delete a message**. Using methods, you can add, modify, and delete messages in the News Feed. Use the [log.blogpost.add](./../log/log-blogpost-add.md) method to add a message to the News Feed on behalf of the current user. You can modify a message using the [log.blogpost.update](./../log/log-blogpost-update.md) method. To delete a message, apply the [log.blogpost.delete](./../log/log-blogpost-delete.md) method.

**How to manage comments**. Use the [log.blogcomment.add](./blogcomment/log-blogcomment-add.md) method to add comments to messages and the [log.blogcomment.delete](./blogcomment/log-blogcomment-delete.md) method to delete comments. The [log.blogcomment.user.get](./blogcomment/log-blogcomment-user-get.md) method returns comments on messages and can filter them by ID range (`FIRST_ID`, `LAST_ID`). If no filtering parameters are provided, the method will return all comments available to the current user, considering their access permissions.

**How to find out which users have read an important message**. A message published in the News Feed can be marked as important. To view the list of users who have read the important message, use the [log.blogpost.getusers.important](./log-blogpost-getusers-important.md) method. The combination of "important message and tracking the list of readers" can be used for automated scenarios of mass informing employees.

**How to get a list of messages**. The [log.blogpost.get](./log-blogpost-get.md) method allows you to access the messages available to the user in the News Feed.

**What events are available**. The events `OnLiveFeedPostAdd`, `OnLiveFeedPostUpdate`, and `OnLiveFeedPostDelete` allow you to track the appearance, modification, and deletion of messages.

## Overview of Methods and Events {#all-methods}

> Scope: [`log`](../scopes/permissions.md)
> 
> Who can execute the method: any user

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [log.blogpost.add](./log-blogpost-add.md) | Adds a message to the News Feed on behalf of the current user ||
    || [log.blogpost.update](./log-blogpost-update.md) | Modifies a message in the News Feed ||
    || [log.blogpost.get](./log-blogpost-get.md) | Retrieves messages available to the user in the News Feed ||
    || [log.blogpost.getusers.important](./log-blogpost-getusers-important.md) | Views users who have read an important message ||
    || [log.blogpost.share](./log-blogpost-share.md) | Adds recipients to a message in the News Feed ||
    || [log.blogpost.delete](./log-blogpost-delete.md) | Deletes a message from the News Feed ||
    || [log.blogcomment.add](./blogcomment/log-blogcomment-add.md) | Adds a comment to a message in the News Feed ||
    || [log.blogcomment.user.get](./blogcomment/log-blogcomment-user-get.md) | Returns a list of comments on a message in the News Feed ||
    || [log.blogcomment.delete](./blogcomment/log-blogcomment-delete.md) | Deletes a comment on a message ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [OnLiveFeedPostAdd](./events/on-live-feed-post-add.md) | When a message is added to the News Feed ||
    || [OnLiveFeedPostDelete](./events/on-live-feed-post-delete.md) | When a message is deleted from the News Feed ||
    || [OnLiveFeedPostUpdate](./events/on-live-feed-post-update.md) | When a message is edited in the News Feed ||
    |#

{% endlist %}