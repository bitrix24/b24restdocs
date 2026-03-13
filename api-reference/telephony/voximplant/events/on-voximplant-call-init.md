# ONVOXIMPLANTCALLINIT Call Initialization Event

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `ONVOXIMPLANTCALLINIT` event is triggered when a call is initialized: either when an incoming call is received or when an outgoing call is initiated.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the handler receives

Data is transmitted as a POST request {.b24-info}

{% list tabs %}

- When using rented numbers or SIP

    ```json
    {
        "event": "ONVOXIMPLANTCALLINIT",
        "event_handler_id": "1057",
        "data": {
            "CALL_ID": "5E316880469A6376.1773306964.8740011",
            "CALL_TYPE": "1",
            "ACCOUNT_SEARCH_ID": "reg150908",
            "PHONE_NUMBER": "+19999999666",
            "CALLER_ID": "reg150908"
        },
        "ts": "1773306964",
        "auth": {
            "access_token": "s7p6eclrvim9da98dt9ch94ekreb52sv",
            "expires_in": "3600",
            "scope": "telephony",
            "domain": "some-domain.bitrix24.com",
            "server_endpoint": "https://oauth.bitrix.info/rest/",
            "status": "F",
            "client_endpoint": "https://example.bitrix24.com/rest/",
            "member_id": "a223c6b9410f85df73e9377d6c4f7213",
            "refresh_token": "4s386p3q0tr8dy09xvmt96234v3dljg8",
            "application_token": "52610fefc120afg4b628cc82d6298cce"
        }
    }
    ```
- When using the telephony application

    ```json
    {
        "event": "ONVOXIMPLANTCALLINIT",
        "event_handler_id": "1057",
        "data": {
            "CALL_ID": "externalCall.7b0c7de811455ef32b18dc5917e4306a.1773239327",
            "CALL_TYPE": "1",
            "CALLER_ID": "+19061234567",
            "REST_APP_ID": "3"
        },
        "ts": "1773239326",
        "auth": {
            "access_token": "s7p6eclrvim9da98dt9ch94ekreb52sv",
            "expires_in": "3600",
            "scope": "telephony",
            "domain": "some-domain.bitrix24.com",
            "server_endpoint": "https://oauth.bitrix.info/rest/",
            "status": "F",
            "client_endpoint": "https://example.bitrix24.com/rest/",
            "member_id": "a223c6b9410f85df73e9377d6c4f7213",
            "refresh_token": "4s386p3q0tr8dy09xvmt96234v3dljg8",
            "application_token": "52610fefc120afg4b628cc82d6298cce"
        }
    }
    ```

{% endlist %}

#|
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`](../../../data-types.md) | Symbolic code of the event.

In this case — `ONVOXIMPLANTCALLINIT` ||
|| **event_handler_id**
[`string`](../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../data-types.md) | Object containing event data.

The structure is described [below](#data) ||
|| **ts**
[`timestamp`](../../../data-types.md) | Date and time the event was sent from the [event queue](../../../events/index.md) ||
|| **auth**
[`object`](../../../data-types.md) | Object containing user authorization parameters under which the event was triggered.

The structure is described [below](#auth) ||
|#

### Data Parameter {#data}

#|
|| **Parameter**
`type` | **Description** ||
|| **CALL_ID**
[`string`](../../../data-types.md) | Identifier of the call ||
|| **CALL_TYPE**
[`integer`](../../../data-types.md) | Type of the call.

Possible values:

- `1` — outgoing
- `2` — incoming
- `3` — incoming with redirection
- `4` — callback
- `5` — informational call ||
|| **ACCOUNT_SEARCH_ID**
[`string`](../../../data-types.md) | Identifier of the line.

Possible values:

- `XXX` — for rented numbers
- `regXXX` — for cloud PBXs
- `sipXXX` — for office PBXs

This field is returned when using rented numbers or SIP ||
|| **PHONE_NUMBER**
[`string`](../../../data-types.md) | The number the operator is calling for an outgoing call, or the number the subscriber is calling for an incoming call.

This field is returned when using rented numbers or SIP ||
|| **CALLER_ID**
[`string`](../../../data-types.md) | Identifier of the line for an outgoing call or the client's phone number for an incoming call ||
|| **REST_APP_ID**
[`integer`](../../../data-types.md) | Identifier of the application associated with the call.

This field is returned when using the telephony application ||
|#

### Auth Parameter {#auth}

{% include notitle [Table with keys in the auth array](../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](./on-voximplant-call-start.md)
- [{#T}](./on-voximplant-call-end.md)
- [{#T}](../../telephony-external-call-register.md)