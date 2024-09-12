# When a visitor fills out the CRM callback form OnExternalCallBackStart

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- the required fields are not specified
- examples are missing

{% endnote %}

{% endif %}

{% include notitle [Scope telephony events](../_includes/scope-telephony-events.md) %}

The event `OnExternalCallBackStart` is triggered when a visitor fills out the CRM callback form. Your application must be selected in the form settings as the line through which the callback will be made.

#|
|| **Field** / **Type** | **Description** ||
|| **PHONE_NUMBER** 
[`string`](../../data-types.md) | Phone number. ||
|| **TEXT** 
[`string`](../../data-types.md) | The text that should be spoken to the user at the start of the call (from the form settings). ||
|| **VOICE** 
[`string`](../../data-types.md) | The identifier of the voice that should speak the text (from the form settings). For a list of voice identifiers, see [voximplant.tts.voices.get](../voximplant/voximplant-tts-voices-get.md). ||
|| **CRM_ENTITY_TYPE** 
[`string`](../../data-types.md) | Type of the related CRM entity. ||
|| **CRM_ENTITY_ID** 
[`integer`](../../data-types.md) | Identifier of the CRM entity, the type of which is specified in CRM_ENTITY_TYPE. ||
|| **LINE_NUMBER** 
[`string`](../../data-types.md) | The number of the external line through which the callback was requested. ||
|#