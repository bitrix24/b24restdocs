# Callback Request Event ONEXTERNALCALLBACKSTART

> Scope: [`telephony`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `ONEXTERNALCALLBACKSTART` is triggered when a visitor fills out the CRM callback form. Your application must be selected as the callback line in the form settings.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the handler receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONEXTERNALCALLBACKSTART",
    "event_handler_id": "1053",
    "data": {
        "PHONE_NUMBER": "+14151234567",
        "TEXT": "A callback has been requested from the website, connecting you with the client",
        "VOICE": "deinternalfemale",
        "CRM_ENTITY_TYPE": "CONTACT",
        "CRM_ENTITY_ID": "5785",
        "LINE_NUMBER": "7"
    },
    "ts": "1773234727",
    "auth": {
        "access_token": "s7p6eclrvim9da28dt9ch94ekreb52sv",
        "expires_in": "3600",
        "scope": "telephony",
        "domain": "some-domain.bitrix24.com",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "status": "F",
        "client_endpoint": "https://example.bitrix24.com/rest/",
        "member_id": "a223c6b9410f85df78e9377d6c4f7213",
        "refresh_token": "4s386p3q0tr8dy89xvmt96234v3dljg8",
        "application_token": "51610fefc120afg4b628cc82d6298cce"
    }
}
```

#|
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`](../../data-types.md) | Symbolic event code.

In this case — `ONEXTERNALCALLBACKSTART` ||
|| **event_handler_id**
[`integer`](../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../data-types.md) | Object containing event data.

The structure is described [below](#data) ||
|| **ts**
[`timestamp`](../../data-types.md) | Date and time the event was sent from the [event queue](../../events/index.md) ||
|| **auth**
[`object`](../../data-types.md) | Object containing the user's authorization parameters under which the event was triggered.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

#|
|| **Parameter**
`type` | **Description** ||
|| **PHONE_NUMBER**
[`string`](../../data-types.md) | The phone number to call back ||
|| **TEXT**
[`string`](../../data-types.md) | The text that should be spoken to the responsible party before the call begins ||
|| **VOICE**
[`string`](../../data-types.md) | Voice identifier for speech synthesis ||
|| **CRM_ENTITY_TYPE**
[`string`](../../data-types.md) | Type of the related CRM object ||
|| **CRM_ENTITY_ID**
[`integer`](../../data-types.md) | Identifier of the CRM object, the type of which is specified in `CRM_ENTITY_TYPE` ||
|| **LINE_NUMBER**
[`string`](../../data-types.md) | The number of the external line through which the callback was requested ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../_includes/auth-params-in-events.md) %}

## Continue exploring

- [{#T}](../voximplant/voximplant-callback-start.md)
- [{#T}](../voximplant/voximplant-tts-voices-get.md)
- [{#T}](./on-external-call-start.md)