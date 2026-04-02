# Telephony

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- How this works in general, the integration via REST, and the management of built-in telephony and connection to the SIP connector.
- Add links to telephony widgets.

{% endnote %}

{% endif %}

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [telephony.externalLine.add](./telephony-external-line-add.md) | Registers an external line ||
    || [telephony.externalLine.update](./telephony-external-line-update.md) | Modifies an external line ||
    || [telephony.externalLine.get](./telephony-external-line-get.md) | Returns a list of external lines ||
    || [telephony.externalLine.delete](./telephony-external-line-delete.md) | Deletes an external line ||
    || [telephony.externalCall.searchCrmEntities](./telephony-external-call-search-crm-entities.md) | Searches for a client in CRM by phone number ||
    || [telephony.externalCall.register](./telephony-external-call-register.md) | Registers the start of a call ||
    || [telephony.externalCall.show](./telephony-external-call-show.md) | Opens the call detail form for the user ||
    || [telephony.externalCall.hide](./telephony-external-call-hide.md) | Hides the call detail form for the user ||
    || [telephony.externalCall.finish](./telephony-external-call-finish.md) | Ends the call ||
    || [telephony.externalCall.attachRecord](./telephony-external-call-attach-record.md) | Attaches a call recording ||
    || [telephony.call.attachTranscription](./telephony-call-attach-transcription.md) | Adds a transcription of the recording to the call ||
    || [voximplant.statistic.get](./voximplant-statistic-get.md) | Returns a list of calls ||
    |#

- Events

    #| 
    || **Event** | **Triggered by** ||
    || [OnExternalCallStart](./events/on-external-call-start.md) | When clicking on a phone number in CRM entities to make an outgoing call ||
    || [OnExternalCallBackStart](./events/on-external-call-back-start.md) | When filling out the CRM form for a callback ||
    |#

{% endlist %}