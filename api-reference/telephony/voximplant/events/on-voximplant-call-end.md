# OnVoximplantCallEnd Event

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

The `OnVoximplantCallEnd` event is triggered when a call ends (recorded in history).

{% note warning %}

Please note that the event will be triggered without authorization data.

{% endnote %}

The event receives an array with the following data:

#|
|| **Parameter** | **Description** ||
|| **CALL_ID** | Call identifier. ||
|| **CALL_TYPE** | Type of call, see description of call types. ||
|| **PHONE_NUMBER** | The number from which the subscriber is calling (if call type is 2 - Incoming) or the number to which the operator is calling (if call type is 1 - Outgoing). ||
|| **ACCOUNT_NUMBER** | The number that received the call (if call type is 2 - Incoming) or the number from which the call was made (if call type is 1 - Outgoing). ||
|| **ACCOUNT_USER_ID** | Identifier of the operator who answered (if call type is 2 - Incoming) or identifier of the calling operator (if call type is 1 - Outgoing). ||
|| **CALL_DURATION** | Duration of the call. ||
|| **CALL_START_DATE** | Date in ISO format. ||
|| **COST** | Cost of the call. ||
|| **COST_CURRENCY** | Currency of the call (USD, EUR). ||
|| **CALL_FAILED_CODE** | Call code, see the table of call codes. ||
|| **CALL_FAILED_REASON** | Text description of the call code (in Latin script). ||
|| **CRM_ACTIVITY_ID** | ID of the CRM activity associated with the call. ||
|#