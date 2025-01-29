# Comments: Overview of Methods

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

A set of methods for working with comments in the timeline of entities.

In fact, comments are the same as [timeline entries](../timeline/index.md), but with a different display and the ability for the user to edit them.

Comment data can be retrieved using the method `rpa.timeline.listForItem` — this method returns all entries, including comments.

#|
|| **Method** | **Description** ||
|| [rpa.comment.add](./rpa-comment-add.md) | Creates a new comment in the timeline of the entity with the identifier `itemId` of the process with the identifier `typeId` ||
|| [rpa.comment.update](./rpa-comment-update.md) | Updates the timeline entry with the identifier `id` ||
|| [rpa.comment.delete](./rpa-comment-delete.md) | Deletes the comment with the identifier `id` ||
|#