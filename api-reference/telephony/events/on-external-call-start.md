# When a user clicks on a phone number in CRM entities to make an outgoing call OnExternalCallStart

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- The mandatory fields are not specified
- Examples are missing

{% endnote %}

{% endif %}

{% include notitle [Scope telephony events](../_includes/scope-telephony-events.md) %}

The `OnExternalCallStart` event is triggered when a user clicks on a phone number in CRM entities to make an outgoing call.

To ensure the event is triggered, go to Telephony > Telephony Settings and select your application in the **Default outgoing call number** field.

Alternatively, you can set this application as the default number in the user settings. The event will only trigger for the selected application.

#|
|| **Field** / **Type** | **Description** ||
|| **USER_ID**
[`integer`](../../data-types.md) | User identifier. ||
|| **PHONE_NUMBER**
[`string`](../../data-types.md) | Outgoing call number. ||
|| **CRM_ENTITY_TYPE**
[`string`](../../data-types.md) | Type of the CRM entity from which the call is made - CONTACT \| COMPANY \| LEAD. ||
|| **CRM_ENTITY_ID**
[`integer`](../../data-types.md) | Identifier of the CRM entity, the type of which is specified in CRM_ENTITY_TYPE. ||
|| **CALL_LIST_ID**
[`integer`](../../data-types.md) | Identifier of the call list, in case the call is made from a call list. ||
|| **LINE_NUMBER**
[`string`](../../data-types.md) | Number of the external line through which the call is requested. ||
|| **CALL_ID**
[`string`](../../data-types.md) | Identifier of the call from the method [telephony.externalcall.register](../telephony-external-call-register.md). ||
|| **IS_MOBILE**
[`integer`](../../data-types.md) | (Y\|N) Indicates whether the call was initiated from a mobile application. ||
|#