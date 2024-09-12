# On Adding a Message to the News Feed

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- what data is passed in the event
- links to pages that have not yet been created are not specified
- examples are missing

{% endnote %}

{% endif %}

{% note info "OnLiveFeedPostAdd" %}

{% include notitle [Scope log all](../_includes/scope-log-all.md) %}

{% endnote %}

The event `OnLiveFeedPostAdd` is triggered after a new post is added to the News Feed. It is a proxy to the event [OnAfterSocNetLogAdd](.).

#|
|| **Field** | **Description** ||
|| **ID** | Identifier of the new message ||
|#
{% include [Footnote on parameters](../../_includes/required.md) %}