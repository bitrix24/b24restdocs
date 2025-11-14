# Comments on News Feed Messages: Overview of Methods

Comments in the News Feed allow users to discuss posts, share opinions, and clarify details. Files can be attached to each comment.

> Quick navigation: [all methods](#all-methods)

## Linking Comments to Other Objects

**News Feed Post**. Comments are linked to a post by the identifier `POST_ID`. The post identifier can be obtained using the method [log.blogpost.get](../log-blogpost-get.md).

**User**. A comment is associated with a user by the identifier `USER_ID`. The user identifier can be retrieved using the methods [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md).

**Files**. Files can be attached to comments. To attach a file, pass an array containing the file name and a string with `Base64` in the `FILES` field. Information about the attached file can be obtained using the method [disk.attachedObject.get](../../disk/attached-object/disk-attached-object-get.md).

{% note tip "Typical use-cases and scenarios" %}

- [How to upload files](../../files/how-to-upload-files.md)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`log`](../../scopes/permissions.md)
>
> Who can perform the method: depending on the method

#|
|| **Method** | **Description** ||
|| [log.blogcomment.add](./log-blogcomment-add.md) | Adds a comment to a News Feed post ||
|| [log.blogcomment.user.get](./log-blogcomment-user-get.md) | Returns a list of comments for a News Feed post ||
|| [log.blogcomment.delete](./log-blogcomment-delete.md) | Deletes a comment from a post ||
|#