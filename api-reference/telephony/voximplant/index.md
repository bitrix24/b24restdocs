# SIP and Built-in Telephony: Overview of Methods

This section describes the workflow for working with built-in telephony and the SIP connector in Bitrix24.

- Built-in telephony is the basic telephony mode in Bitrix24 for handling calls within the platform. Through the API in this mode, you can initiate calls, receive call events, manage outgoing lines, and configure users' SIP settings.
- The SIP connector is the integration mode of Bitrix24 with external SIP providers and PBXs. Through the API in this mode, you can create, update, and delete SIP connections for the application, check their registration status, and assign a default SIP line for outgoing calls.

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [How to choose and connect a SIP PBX in Bitrix24](https://helpdesk.bitrix24.com/open/19721438/)

## How to Choose a Section

#|
|| **If you need** | **Open the section** ||
|| React to the start and end of calls | [Events](./events/index.md) ||
|| Manage SIP connections for the application | [Managing SIP Connections](./sip/index.md) ||
|| Manage default outgoing lines | [Managing Lines](./lines/index.md) ||
|| Retrieve user SIP settings and enable SIP devices | [Managing Users](./users/index.md) ||
|#

## Interaction with Other Objects

**User.** To configure SIP settings for an employee, you need the user identifier `USER_ID`. You can obtain `USER_ID` using the [user.get](../../user/user-get.md) method.

**SIP Connection.** To work with external SIP configuration, you need the SIP connection configuration identifier `CONFIG_ID`. This identifier can be obtained in the response from [voximplant.sip.add](./sip/voximplant-sip-add.md) and in the list from [voximplant.sip.get](./sip/voximplant-sip-get.md).

**Outgoing Line.** In outgoing call scenarios, the identifier for the outgoing line `LINE_ID` is used. You can obtain `LINE_ID` using the [voximplant.line.get](./lines/voximplant-line-get.md) method.

## Access Permissions

Access to telephony methods depends on the user's permissions and role in Bitrix24. If permissions are insufficient, the method will return an access error.

Specific permission requirements can be found in the description of each method.

{% note tip "User Documentation" %}

- [How to configure access permissions in telephony](https://helpdesk.bitrix24.com/open/18216960/)

{% endnote %}

## How to Get Started

1. Choose what you will be working with: outgoing call, SIP connection, outgoing lines, user SIP settings, or call events.
2. For an outgoing call, use [voximplant.callback.start](./voximplant-callback-start.md), [voximplant.infocall.startwithsound](./voximplant-infocall-start-with-sound.md), or [voximplant.infocall.startwithtext](./voximplant-infocall-start-with-text.md).
3. For a SIP scenario, create a connection using [voximplant.sip.add](./sip/voximplant-sip-add.md), then check it using [voximplant.sip.get](./sip/voximplant-sip-get.md) and [voximplant.sip.status](./sip/voximplant-sip-status.md).
4. Assign an outgoing line using [voximplant.line.outgoing.set](./lines/voximplant-line-outgoing-set.md) or [voximplant.line.outgoing.sip.set](./lines/voximplant-line-outgoing-sip-set.md). After [voximplant.line.outgoing.set](./lines/voximplant-line-outgoing-set.md), verify the actual line using the [voximplant.line.outgoing.get](./lines/voximplant-line-outgoing-get.md) method.
5. To work with employees' SIP settings, use [voximplant.user.get](./users/voximplant-user-get.md) and [voximplant.user.activatePhone](./users/voximplant-user-activate-phone.md).
6. Subscribe to events in the [Events](./events/index.md) section if you need to handle the call lifecycle.

## Overview of Methods and Events {#all-methods}

> Scope: [`telephony`](../../scopes/permissions.md)
>
> Who can execute methods: depending on the method

### Main

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [voximplant.callback.start](./voximplant-callback-start.md) | Initiates a callback ||
    || [voximplant.infocall.startwithsound](./voximplant-infocall-start-with-sound.md) | Initiates an auto call and plays an MP3 file from a URL ||
    || [voximplant.infocall.startwithtext](./voximplant-infocall-start-with-text.md) | Initiates an auto call and plays a specified text to the recipient using speech synthesis ||
    || [voximplant.tts.voices.get](./voximplant-tts-voices-get.md) | Returns a list of available voices for speech synthesis ||
    || [voximplant.statistic.get](./voximplant-statistic-get.md) | Returns a list of calls ||
    || [voximplant.url.get](./voximplant-url-get.md) | Returns links for navigating through telephony pages ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [OnVoximplantCallInit](./events/on-voximplant-call-init.md) | When a call is initialized manually or via methods [voximplant.callback.start](./voximplant-callback-start.md), [voximplant.infocall.startwithsound](./voximplant-infocall-start-with-sound.md), [voximplant.infocall.startwithtext](./voximplant-infocall-start-with-text.md), [telephony.externalCall.register](../telephony-external-call-register.md) ||
    || [OnVoximplantCallStart](./events/on-voximplant-call-start.md) | When a conversation starts: when the operator answers an incoming call or the recipient answers an outgoing call ||
    || [OnVoximplantCallEnd](./events/on-voximplant-call-end.md) | When a conversation ends and is recorded in history or via the method [telephony.externalCall.finish](../telephony-external-call-finish.md) ||
    |#

{% endlist %}

### Managing SIP Connections

#|
|| **Method** | **Description** ||
|| [voximplant.sip.add](./sip/voximplant-sip-add.md) | Creates a SIP connection linked to the application ||
|| [voximplant.sip.update](./sip/voximplant-sip-update.md) | Updates an existing SIP connection ||
|| [voximplant.sip.get](./sip/voximplant-sip-get.md) | Returns a list of SIP connections created by the application ||
|| [voximplant.sip.status](./sip/voximplant-sip-status.md) | Returns the SIP registration status for the cloud PBX ||
|| [voximplant.sip.delete](./sip/voximplant-sip-delete.md) | Deletes an existing SIP connection ||
|| [voximplant.sip.connector.status](./sip/voximplant-sip-connector-status.md) | Returns the current status of the SIP connector ||
|#

### Managing Lines

#|
|| **Method** | **Description** ||
|| [voximplant.line.get](./lines/voximplant-line-get.md) | Returns a list of available outgoing lines ||
|| [voximplant.line.outgoing.get](./lines/voximplant-line-outgoing-get.md) | Returns the identifier of the current default outgoing line ||
|| [voximplant.line.outgoing.set](./lines/voximplant-line-outgoing-set.md) | Sets the default outgoing line ||
|| [voximplant.line.outgoing.sip.set](./lines/voximplant-line-outgoing-sip-set.md) | Sets the default outgoing SIP line ||
|#

### Managing Users

#|
|| **Method** | **Description** ||
|| [voximplant.user.get](./users/voximplant-user-get.md) | Returns user settings ||
|| [voximplant.user.activatePhone](./users/voximplant-user-activate-phone.md) | Sets the employee's SIP device presence flag ||
|#