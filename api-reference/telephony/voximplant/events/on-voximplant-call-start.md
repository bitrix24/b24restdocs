# OnVoximplantCallStart Event

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing

{% endnote %}

{% endif %}

{% include notitle [Scope of telephony events](../../_includes/scope-telephony-events.md) %}

The `OnVoximplantCallStart` event is triggered when a call begins (when the operator answers an incoming call and when the subscriber answers an outgoing call).

{% note warning %}

Please note that the event will be triggered without authorization data.

{% endnote %}

The event receives an array with the following data:

#|
|| **Parameter** | **Description** ||
|| **CALL_ID** | Call identifier. ||
|| **USER_ID** | Identifier of the user who answered. ||
|#