# Using SIP and Built-in Telephony

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- An introductory note about the built-in telephony and SIP connector, detailing which scenarios are available through REST in these telephony options.

{% endnote %}

{% endif %}

When creating applications, keep in mind that methods are available to users according to the specified [access permissions](https://helpdesk.bitrix24.com/open/18177766/). The required permission level is indicated in the method description.

{% endnote %}

{% list tabs %}

- Methods

    #| 
    || [voximplant.callback.start](./voximplant-callback-start.md) | Initiates a callback ||
    || [voximplant.infocall.startwithsound](./voximplant-infocall-start-with-sound.md) | Makes a call to the specified number while playing an mp3 file from a URL ||
    || [voximplant.infocall.startwithtext](./voximplant-infocall-start-with-text.md) | Makes a call to the specified number with automatic reading of the given text ||
    || [voximplant.tts.voices.get](./voximplant-tts-voices-get.md) | Retrieves an array of available voices for speech synthesis ||
    || [voximplant.url.get](./voximplant-url-get.md) | Retrieves a set of links for navigation through telephony pages ||
    |#

- Events

    #| 
    || **Event** | **Triggered** ||
    || [OnVoximplantCallInit](./events/on-voximplant-call-init.md) | When a call is initialized (incoming or outgoing call begins). ||
    || [OnVoximplantCallStart](./events/on-voximplant-call-start.md) | When the conversation starts (operator answers an incoming call or subscriber answers an outgoing call). ||
    || [OnVoximplantCallEnd](./events/on-voximplant-call-end.md) | When the conversation ends (recorded in history). ||
    |#

{% endlist %}

## User Management

#| 
|| [voximplant.user.activatePhone](./users/voximplant-user-activate-phone.md) | Sets the employee's SIP phone presence ||
|| [voximplant.user.deactivatePhone](./users/voximplant-user-deactivate-phone.md) | Disables the employee's SIP phone presence ||
|| [voximplant.user.get](./users/voximplant-user-get.md) | Retrieves user settings ||
|#

## Managing SIP Connections

#| 
|| [voximplant.sip.add](./sip/voximplant-sip-add.md) | Creates a new SIP line linked to the application ||
|| [voximplant.sip.connector.status](./sip/voximplant-sip-connector-status.md) | Retrieves the current status of the SIP Connector ||
|| [voximplant.sip.delete](./sip/voximplant-sip-delete.md) | Deletes an existing SIP line ||
|| [voximplant.sip.get](./sip/voximplant-sip-get.md) | Retrieves a list of all SIP lines created by the application ||
|| [voximplant.sip.status](./sip/voximplant-sip-status.md) | Retrieves the current status of SIP registration (only for cloud PBXs) ||
|| [voximplant.sip.update](./sip/voximplant-sip-update.md) | Modifies an existing SIP line ||
|#

# Managing Lines

#| 
|| [voximplant.line.get](./lines/voximplant-line-get.md) | Retrieves a list of all available outgoing lines ||
|| [voximplant.line.outgoing.get](./lines/voximplant-line-outgoing-get.md) | Retrieves the currently selected line as the default outgoing line ||
|| [voximplant.line.outgoing.set](./lines/voximplant-line-outgoing-set.md) | Sets the selected line as the default outgoing line ||
|| [voximplant.line.outgoing.sip.set](./lines/voximplant-line-outgoing-sip-set.md) | Sets the selected SIP line as the default outgoing line ||
|#