# OnLiveFeedPostDelete Message Deletion

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- what data is sent in the event
- links to pages that have not yet been created are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`log`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `OnLiveFeedPostDelete` event is triggered after a post is deleted from the News Feed. It is a proxy to the [OnSocNetLogDelete](.).

#|
|| **Field** | **Description** ||
|| **ID** | Identifier of the deleted message ||
|#
{% include [Footnote about parameters](../../_includes/required.md) %}