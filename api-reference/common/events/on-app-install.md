# Event after Successful Application Installation OnAppInstall

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing

{% endnote %}

{% endif %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `OnAppInstall` event is triggered immediately after the application is successfully installed on Bitrix24. The **application_token** is passed to the handler, which is important to save (see [{#T}](../../events/safe-event-handlers.md)).

## Parameters

No parameters

{% note warning %}

 The handler for this event can be set in the installation script of the application (which is specified in the version card in a separate field).

{% endnote %}