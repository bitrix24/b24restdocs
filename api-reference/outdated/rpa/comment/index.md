# Overview of Methods

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

A set of methods for working with comments in the timeline of elements.

In fact, comments are the same as [timeline entries](../timeline/index.md), but with a different display and the ability for the user to edit them.

Comment data can be retrieved using the `rpa.timeline.listForItem` method — this method returns all entries, including comments.

#|
|| **Method** | **Description** ||
|| [rpa.comment.add](./rpa-comment-add.md) | This method will create a new comment in the timeline of the element with the identifier itemId of the process with the identifier typeId. ||
|| [rpa.comment.update](./rpa-comment-update.md) | This method will update the timeline entry with the identifier id. ||
|| [rpa.comment.delete](./rpa-comment-delete.md) | This method will delete the comment with the identifier id. ||
|#