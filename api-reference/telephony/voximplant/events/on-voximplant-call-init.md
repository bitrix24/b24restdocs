# OnVoximplantCallInit Event Initialization

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing

{% endnote %}

{% endif %}

{% include notitle [Scope of telephony events](../../_includes/scope-telephony-events.md) %}

The `OnVoximplantCallInit` event is triggered when a call is initialized (upon receiving or starting an outgoing call).

{% note warning %}

Please note that the event will be triggered without authorization data.

{% endnote %}

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

An array of data is received in the event:

#|
|| **Parameter** | **Description** ||
|| **CALL_ID** | Call identifier. ||
|| **CALL_TYPE** | Type of call, see description of call types. ||
|| **ACCOUNT_SEARCH_ID** | Line identifier - numeric for rented lines, regXXX for Cloud PBXs, sipXXX for Office PBXs. ||
|| **PHONE_NUMBER** | The number the operator is calling (if call type: 1 - Outgoing) or the number the subscriber is calling (if call type: 2 - Incoming). ||
|| **CALLER_ID** | Line identifier (if call type: 1 - Outgoing) or the phone number that called the account (if call type: 2 - Incoming). ||
|#