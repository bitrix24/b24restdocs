# Event onCrmActivityDelete

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: `any user`

The event `onCrmActivityDelete` is triggered when a deal is deleted in the CRM timeline.

## What the handler receives

Data is sent as a POST request {.b24-info}

```json
{
    "event": "onCrmActivityDelete",
    "data": {
        "FIELDS": {
            "ID": "999"
        }
    },
    "ts": "1466439714",
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": "3600",
        "scope": "crm",
        "domain": "some-domain.bitrix24.com",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "status": "L",
        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
        "member_id": "a223c6b3710f85df22e9377d6c4f7553",
        "refresh_token": "4s386p3q0tr8dy89xvmt96234v3dljg8",
        "application_token": "51856fefc120afa4b628cc82d3935cce"
    }
}
```

#|
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`](../../../data-types.md) | Symbolic code of the event. In our case, it is `onCrmActivityDelete` ||
|| **data**
`array` | An object containing information about the deleted deal.

Contains a single key `FIELDS` ||
|| **ts**
[`timestamp`](../../../data-types.md) | Date and time the event was sent from the [event queue](../../../../events/index.md) ||
|| **auth**
[`array`](../../../data-types.md) | Authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter FIELDS {#fields}

#|
|| **Parameter**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | `ID` with the value of the deleted deal's identifier ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../../../_includes/auth-params-in-events.md) %}

## Continue your exploration 

- [{#T}](../../../../events/index.md)
- [{#T}](../../../../events/event-bind.md)
- [{#T}](./on-crm-activity-add.md)
- [{#T}](./on-crm-activity-update.md)