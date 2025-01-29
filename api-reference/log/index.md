# News Feed (formerly "Live Feed")

Using the methods and events of the REST API for the [news feed in Bitrix24](https://helpdesk.bitrix24.com/open/18636162/), you can manage messages (add, modify, and delete them), as well as add comments to messages.

> Quick navigation: [all methods and events](#all-methods)

## Private User Scenarios

You will be able to automatically publish various types of operational information in the News Feed:

- Sales department metrics and key business metrics
- Project summaries, current KPIs of departments
- New documents for discussion
- Various informers (exchange rates, changes in the operating hours of retail outlets, etc.)
- Congratulations to top employees

Using the combination of "important message" + tracking the [list of readers](log-blogpost-getusers-important.md) can be utilized for automated scenarios of mass informing employees about various regulations, changes in the company, etc.

Tracking events for [new message appearance](events/index.md) or [message updates](events/on-live-feed-post-update.md) in the news feed will allow for quick responses to new information within Bitrix24, for example, for automatic knowledge base formation, message synchronization with external systems, tracking communications within departments, etc.

## Functional Features

Each message in the news feed can be addressed to a group of recipients:

- specific [users](../user/index.md)
- [departments](../departments/index.md) of the company
- participants of [working groups/projects](../sonet-group/sonet-group-create.md)

Added messages will be visible directly on the News Feed page in Bitrix24 to those users who were specified as recipients (individually or through their departments), as well as in the news feeds of working groups.

The published message can be marked as "important." Such messages are displayed in a special way, and a [special method](log-blogpost-getusers-important.md) will allow tracking users who have read such a message.


## Recommended Learning Sequence

- How to [add a new message](log-blogpost-add.md) and how to [modify an existing one](log-blogpost-update.md)
- How to get a [list of messages](log-blogpost-get.md)
- What [events](events/index.md) are available
- How to [add a comment](log-blogcomment-add.md) to a message

## Overview of Methods and Events {#all-methods}

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [log.blogcomment.add](./log-blogcomment-add.md) | Adds a comment to a News Feed message ||
    || [log.blogpost.add](./log-blogpost-add.md) | Adds a message to the News Feed on behalf of the current user ||
    || [log.blogpost.update](./log-blogpost-update.md) | Modifies a News Feed message ||
    || [log.blogpost.get](./log-blogpost-get.md) | Retrieves messages available to the user in the News Feed ||
    || [log.blogpost.getusers.important](./log-blogpost-getusers-important.md) | Views users who have read an important message ||
    || [log.blogpost.share](./log-blogpost-share.md) | Adds recipients to a News Feed message ||
    || [log.blogpost.delete](./log-blogpost-delete.md) | Deletes a message from the News Feed ||
    |#

- Events

    #| 
    || **Event** | **Description** ||
    || [OnLiveFeedPostAdd](./events/on-live-feed-post-add.md) | On adding a message to the News Feed ||
    || [OnLiveFeedPostDelete](./events/on-live-feed-post-delete.md) | On deleting a message from the News Feed ||
    || [OnLiveFeedPostUpdate](./events/on-live-feed-post-update.md) | On editing a message in the News Feed ||
    |#

{% endlist %}