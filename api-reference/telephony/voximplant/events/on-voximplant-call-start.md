# OnVoximplantCallStart Event

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing

{% endnote %}

{% endif %}

{% include notitle [Scope of telephony events](../../_includes/scope-telephony-events.md) %}

The `OnVoximplantCallStart` event is triggered at the beginning of a call (when the operator answers an incoming call and when the subscriber answers an outgoing call).

{% note warning %}

Please note that the event will be triggered without authorization data.

{% endnote %}

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

The event receives an array with the following data:

#|
|| **Parameter** | **Description** ||
|| **CALL_ID** | Call identifier. ||
|| **USER_ID** | Identifier of the user who answered. ||
|#