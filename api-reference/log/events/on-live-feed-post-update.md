# On Editing a Message in the News Feed OnLiveFeedPostUpdate

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- what data is passed in the event
- links to pages that have not yet been created are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`log`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `OnLiveFeedPostUpdate` event is triggered after a post in the News Feed is modified. Proxy to the event [OnAfterSocNetLogUpdate](.).

#|
|| **Field** | **Description** ||
|| **ID** | Identifier of the modified message ||
|#
{% include [Footnote on parameters](../../_includes/required.md) %}