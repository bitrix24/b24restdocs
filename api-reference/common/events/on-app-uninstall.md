# Event on Application Uninstallation onAppUninstall

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- missing parameters or fields
- parameter types not specified
- parameter requirements not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `onAppUnistall` event is triggered when an application is uninstalled.

## Event Parameters:

#|
|| **Parameter** | **Description** ||
|| **CLEAN=1/0**
[`unknown`](../../data-types.md) | The value of the "clear application data" flag set by the user when uninstalling the application. ||
|| **application_token**
[`unknown`](../../data-types.md) | The secret identifier (see [{#T}](../../events/safe-event-handlers.md)). ||
|#

{% note warning %}

When an application is uninstalled, all access permissions for the application to the REST API are revoked. Therefore, even though the event handler will receive authorization data, it can no longer use the API on behalf of the uninstalled application.

{% endnote %}