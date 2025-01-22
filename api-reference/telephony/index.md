# Telephony

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- how this works, what integration through REST is, and how to manage built-in telephony and connect to a SIP connector.

{% endnote %}

{% endif %}

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [telephony.externalCall.searchCrmEntities](./telephony-external-call-search-crm-entities.md) | Retrieves CRM entities by phone number ||
    || [telephony.externalcall.register](./telephony-external-call-register.md) | Registers a new call ||
    || [telephony.externalcall.finish](./telephony-external-call-finish.md) | Ends a call ||
    || [telephony.externalcall.show](./telephony-external-call-show.md) | Displays the call card to the user ||
    || [telephony.externalcall.hide](./telephony-external-call-hide.md) | Hides the call card ||
    || [telephony.call.attachTranscription](./telephony-call-attach-transcription.md) | Attaches a transcription of the recording to the call ||
    || [telephony.externalCall.attachRecord](./telephony-external-call-attach-record.md) | Attaches a recording of the conversation to the call ||
    || [telephony.externalLine.add](./telephony-external-line-add.md) | Adds an external line ||
    || [telephony.externalLine.update](./telephony-external-line-update.md) | Updates information about an external line ||
    || [telephony.externalLine.get](./telephony-external-line-get.md) | Retrieves information about an external line ||
    || [telephony.externalLine.delete](./telephony-external-line-delete.md) | Deletes an external line ||
    || [voximplant.statistic.get](./voximplant-statistic-get.md) | Returns a list of call history ||
    |#

- Events

    #| 
    || **Event** | **Triggered** ||
    || [OnExternalCallStart](./events/on-external-call-start.md) | When clicking on a phone number in CRM entities to make an outgoing call ||
    || [OnExternalCallBackStart](./events/on-external-call-back-start.md) | When filling out the CRM form for a callback ||
    |#

{% endlist %}