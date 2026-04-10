# Telephony: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Bitrix24 telephony helps manage calls within the CRM: making and receiving calls, maintaining call history, linking calls to client records, and saving recordings.

The REST API supports two scenarios for working with telephony:

- Integration of external telephony — the application registers the line, reports the call, manages the record, and ends the call.
- Management of built-in telephony and SIP connector — the application works with SIP connections, outgoing lines, users, and call events.

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [How to start calling from Bitrix24: choosing a telephony connection method](https://helpdesk.bitrix24.com/open/25816375/)

## How to Choose a Section

#|
|| **If you need** | **Open the section** ||
|| Integrate external telephony via REST | [External Telephony Methods](#external-telephony) ||
|| Manage SIP connections, lines, and SIP settings for employees | [SIP and Built-in Telephony](./voximplant/index.md) ||
|| Subscribe to external telephony events | [Events](./events/index.md) ||
|#

## Connection with Other Objects

**User.** The identifier `USER_ID` links the call and SIP settings to the employee. You can obtain `USER_ID` using the [user.get](../user/user-get.md) method. Pass `USER_ID` to [telephony.externalCall.show](./telephony-external-call-show.md) and methods in the [User Management](./voximplant/users/index.md) section.

**Call.** The identifier `CALL_ID` is created in [telephony.externalCall.register](./telephony-external-call-register.md) and is used in [telephony.externalCall.show](./telephony-external-call-show.md), [telephony.externalCall.hide](./telephony-external-call-hide.md), [telephony.externalCall.finish](./telephony-external-call-finish.md), [telephony.externalCall.attachRecord](./telephony-external-call-attach-record.md), and [telephony.call.attachTranscription](./telephony-call-attach-transcription.md).

**CRM.** The method [telephony.externalCall.searchCrmEntities](./telephony-external-call-search-crm-entities.md) searches for CRM entities by phone number to link the call to the client record.

**Line and SIP Connection.** For built-in telephony scenarios, `LINE_ID` and `CONFIG_ID` are used. You can obtain `LINE_ID` using the [voximplant.line.get](./voximplant/lines/voximplant-line-get.md) method, and `CONFIG_ID` using the [voximplant.sip.add](./voximplant/sip/voximplant-sip-add.md) and [voximplant.sip.get](./voximplant/sip/voximplant-sip-get.md) methods. For complete context, refer to the sections [Managing Lines](./voximplant/lines/index.md) and [Managing SIP Connections](./voximplant/sip/index.md).

## How to Start Working with External Telephony

1. Register a line using the [telephony.externalLine.add](./telephony-external-line-add.md) method.
2. When starting a call, invoke [telephony.externalCall.register](./telephony-external-call-register.md).
3. If necessary, open the call record via [telephony.externalCall.show](./telephony-external-call-show.md).
4. After the call ends, invoke [telephony.externalCall.finish](./telephony-external-call-finish.md) and pass the recording via [telephony.externalCall.attachRecord](./telephony-external-call-attach-record.md).
5. Subscribe to [OnExternalCallStart](./events/on-external-call-start.md) and [OnExternalCallBackStart](./events/on-external-call-back-start.md) if you need to handle call initiation from the CRM interface.

## How to Start Working with Built-in Telephony and SIP Connector

1. Create a SIP connection using the [voximplant.sip.add](./voximplant/sip/voximplant-sip-add.md) method.
2. Check the connection using the [voximplant.sip.get](./voximplant/sip/voximplant-sip-get.md) and [voximplant.sip.status](./voximplant/sip/voximplant-sip-status.md) methods.
3. Obtain a list of available lines using the [voximplant.line.get](./voximplant/lines/voximplant-line-get.md) method.
4. Set the default outgoing line using [voximplant.line.outgoing.set](./voximplant/lines/voximplant-line-outgoing-set.md) or [voximplant.line.outgoing.sip.set](./voximplant/lines/voximplant-line-outgoing-sip-set.md).
5. For employees, obtain SIP settings using the [voximplant.user.get](./voximplant/users/voximplant-user-get.md) method and, if necessary, activate the SIP device using [voximplant.user.activatePhone](./voximplant/users/voximplant-user-activate-phone.md).
6. Subscribe to events [OnVoximplantCallInit](./voximplant/events/on-voximplant-call-init.md), [OnVoximplantCallStart](./voximplant/events/on-voximplant-call-start.md), [OnVoximplantCallEnd](./voximplant/events/on-voximplant-call-end.md) if you need to handle the call lifecycle.

## Widgets

You can embed an application into the call record and display the operator interface right during the conversation.

- [Tab in the call record CALL_CARD](../widgets/telephony/index.md)
- [JS interface for the call record](../widgets/ui-interaction/call-card/index.md)

## Limitations and Checks

- The methods [telephony.externalCall.register](./telephony-external-call-register.md) and [telephony.externalCall.finish](./telephony-external-call-finish.md) work only in the context of an [application](../../settings/app-installation/index.md).
- In [telephony.externalCall.register](./telephony-external-call-register.md), pass a unique `EXTERNAL_CALL_ID` for each physical call to avoid receiving an existing `CALL_ID` when re-registering within 30 minutes.
- Call [telephony.externalCall.attachRecord](./telephony-external-call-attach-record.md) after [telephony.externalCall.finish](./telephony-external-call-finish.md) when the recording is ready.
- Call [telephony.call.attachTranscription](./telephony-call-attach-transcription.md) for a completed call.
- After [voximplant.line.outgoing.set](./voximplant/lines/voximplant-line-outgoing-set.md), check the actual line using [voximplant.line.outgoing.get](./voximplant/lines/voximplant-line-outgoing-get.md).

## Overview of Methods and Events {#all-methods}

> Scope: [`telephony`](../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### External Telephony {#external-telephony}

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [telephony.externalLine.add](./telephony-external-line-add.md) | Registers an external line ||
    || [telephony.externalLine.update](./telephony-external-line-update.md) | Modifies an external line ||
    || [telephony.externalLine.get](./telephony-external-line-get.md) | Returns a list of external lines ||
    || [telephony.externalLine.delete](./telephony-external-line-delete.md) | Deletes an external line ||
    || [telephony.externalCall.searchCrmEntities](./telephony-external-call-search-crm-entities.md) | Searches CRM client entities by phone number ||
    || [telephony.externalCall.register](./telephony-external-call-register.md) | Registers the start of a call ||
    || [telephony.externalCall.show](./telephony-external-call-show.md) | Opens the call record for the user ||
    || [telephony.externalCall.hide](./telephony-external-call-hide.md) | Hides the call record for the user ||
    || [telephony.externalCall.finish](./telephony-external-call-finish.md) | Ends the call ||
    || [telephony.externalCall.attachRecord](./telephony-external-call-attach-record.md) | Attaches the call recording ||
    || [telephony.call.attachTranscription](./telephony-call-attach-transcription.md) | Adds a transcription of the recording to the call ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [OnExternalCallStart](./events/on-external-call-start.md) | When clicking on a phone number in CRM entities to make an outgoing call ||
    || [OnExternalCallBackStart](./events/on-external-call-back-start.md) | When filling out the CRM callback form ||
    |#

{% endlist %}

### SIP and Built-in Telephony

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [voximplant.callback.start](./voximplant/voximplant-callback-start.md) | Initiates a callback ||
    || [voximplant.infocall.startwithsound](./voximplant/voximplant-infocall-start-with-sound.md) | Initiates an auto-call and plays an MP3 file from a URL ||
    || [voximplant.infocall.startwithtext](./voximplant/voximplant-infocall-start-with-text.md) | Initiates an auto-call and reads the specified text to the recipient using speech synthesis ||
    || [voximplant.tts.voices.get](./voximplant/voximplant-tts-voices-get.md) | Returns a list of available voices for speech synthesis ||
    || [voximplant.url.get](./voximplant/voximplant-url-get.md) | Returns links for navigating telephony pages ||
    || [voximplant.statistic.get](./voximplant/voximplant-statistic-get.md) | Returns a list of calls ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [OnVoximplantCallInit](./voximplant/events/on-voximplant-call-init.md) | When initializing a call manually or via methods [voximplant.callback.start](./voximplant/voximplant-callback-start.md), [voximplant.infocall.startwithsound](./voximplant/voximplant-infocall-start-with-sound.md), [voximplant.infocall.startwithtext](./voximplant/voximplant-infocall-start-with-text.md), [telephony.externalCall.register](./telephony-external-call-register.md) ||
    || [OnVoximplantCallStart](./voximplant/events/on-voximplant-call-start.md) | When the conversation starts: operator answers on incoming and recipient answers on outgoing calls ||
    || [OnVoximplantCallEnd](./voximplant/events/on-voximplant-call-end.md) | When the conversation ends and is recorded in history or via the [telephony.externalCall.finish](./telephony-external-call-finish.md) method ||
    |#

{% endlist %}

### Managing SIP Connections

#|
|| **Method** | **Description** ||
|| [voximplant.sip.add](./voximplant/sip/voximplant-sip-add.md) | Creates a SIP connection linked to the application ||
|| [voximplant.sip.update](./voximplant/sip/voximplant-sip-update.md) | Updates an existing SIP connection ||
|| [voximplant.sip.get](./voximplant/sip/voximplant-sip-get.md) | Returns a list of SIP connections created by the application ||
|| [voximplant.sip.status](./voximplant/sip/voximplant-sip-status.md) | Returns the status of SIP registration for the cloud PBX ||
|| [voximplant.sip.delete](./voximplant/sip/voximplant-sip-delete.md) | Deletes an existing SIP connection ||
|| [voximplant.sip.connector.status](./voximplant/sip/voximplant-sip-connector-status.md) | Returns the current status of the SIP connector ||
|#

### Managing Lines

#|
|| **Method** | **Description** ||
|| [voximplant.line.get](./voximplant/lines/voximplant-line-get.md) | Returns a list of available outgoing lines ||
|| [voximplant.line.outgoing.get](./voximplant/lines/voximplant-line-outgoing-get.md) | Returns the identifier of the current default outgoing line ||
|| [voximplant.line.outgoing.set](./voximplant/lines/voximplant-line-outgoing-set.md) | Sets the default outgoing line ||
|| [voximplant.line.outgoing.sip.set](./voximplant/lines/voximplant-line-outgoing-sip-set.md) | Sets the default outgoing SIP line ||
|#

### Managing Users

#|
|| **Method** | **Description** ||
|| [voximplant.user.get](./voximplant/users/voximplant-user-get.md) | Returns user settings ||
|| [voximplant.user.activatePhone](./voximplant/users/voximplant-user-activate-phone.md) | Sets the employee's SIP device presence flag ||
|#