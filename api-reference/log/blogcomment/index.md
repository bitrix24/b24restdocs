# Comments on News Feed Messages: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The News Feed is a shared feed of employee posts in Bitrix24, similar to an internal social network. Users leave comments under a post to discuss it, share opinions, and clarify details. Files can be attached to a comment.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Feed](https://helpdesk.bitrix24.com/section/47478/)

## Linking Comments to Other Objects

**News Feed Post.** A comment is linked to a post by the identifier `POST_ID`. The [log.blogpost.get](../log-blogpost-get.md) method returns the post identifier.

**User.** A comment is linked to a user by the identifier `USER_ID`. The [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods return the user identifier.

**Files.** To attach a file to a comment, pass an array in the `FILES` parameter containing the file name and the file content encoded in `Base64` as a string. The [disk.attachedObject.get](../../disk/attached-object/disk-attached-object-get.md) method returns information about the attached file.

{% note tip "Typical use-cases and scenarios" %}

- [How to upload files](../../files/how-to-upload-files.md)

{% endnote %}

## How to Get Started

1. Retrieve the post identifier `POST_ID` using the [log.blogpost.get](../log-blogpost-get.md) method
2. Add a comment using the [log.blogcomment.add](./log-blogcomment-add.md) method
3. Retrieve the post comments using the [log.blogcomment.get](./log-blogcomment-get.md) method. The [log.blogcomment.user.get](./log-blogcomment-user-get.md) method returns the comments of a single user on all posts
4. Delete the comment using the [log.blogcomment.delete](./log-blogcomment-delete.md) method. The comment author or an administrator can delete it

## Overview of Methods {#all-methods}

> Scope: [`log`](../../scopes/permissions.md)
>
> Who can perform the method: depending on the method

#|
|| **Method** | **Description** ||
|| [log.blogcomment.add](./log-blogcomment-add.md) | Adds a comment to a News Feed post ||
|| [log.blogcomment.get](./log-blogcomment-get.md) | Returns comments on a News Feed post ||
|| [log.blogcomment.user.get](./log-blogcomment-user-get.md) | Returns a user's comments on News Feed posts after or before a specified comment identifier ||
|| [log.blogcomment.delete](./log-blogcomment-delete.md) | Deletes a comment on a News Feed post ||
|#
