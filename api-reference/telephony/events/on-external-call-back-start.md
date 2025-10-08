# When a visitor fills out the CRM callback form OnExternalCallBackStart

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required fields are not specified
- examples are missing

{% endnote %}

{% endif %}

{% include notitle [Scope telephony events](../_includes/scope-telephony-events.md) %}

The event `OnExternalCallBackStart` is triggered when a visitor fills out the CRM callback form. In the form settings, your application must be selected as the line through which the callback will be made.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

#|
|| **Field** / **Type** | **Description** ||
|| **PHONE_NUMBER** 
[`string`](../../data-types.md) | Phone number. ||
|| **TEXT** 
[`string`](../../data-types.md) | Text that should be spoken to the user at the start of the call (from the form settings). ||
|| **VOICE** 
[`string`](../../data-types.md) | Identifier of the voice that should speak the text (from the form settings). For a list of voice identifiers, see [voximplant.tts.voices.get](../voximplant/voximplant-tts-voices-get.md). ||
|| **CRM_ENTITY_TYPE** 
[`string`](../../data-types.md) | Type of the related CRM entity. ||
|| **CRM_ENTITY_ID** 
[`integer`](../../data-types.md) | Identifier of the CRM entity, the type of which is specified in CRM_ENTITY_TYPE. ||
|| **LINE_NUMBER** 
[`string`](../../data-types.md) | Number of the external line through which the callback was requested. ||
|#