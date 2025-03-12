# Event for Updating the Regular Deal Template onCrmDealRecurringUpdate

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `ONCRMDEALRECURRINGUPDATE` will trigger when the template of a regular deal is updated.

## What the Handler Receives

Data is sent as a POST request {.b24-info}

```json
{
    "event": "ONCRMDEALRECURRINGUPDATE",
    "event_handler_id": "679",
    "data": {
        "FIELDS": {
            "ID": "9",
            "RECURRING_DEAL_ID": "6791"
        }
    },
    "ts": "1741092489",
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
[`string`](../../../../data-types.md) | Symbolic code of the event.

In this case — `ONCRMDEALRECURRINGUPDATE`||
|| **event_handler_id**
[`integer`](../../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../../data-types.md) | Object containing information about the modified template.

Contains a single key `FIELDS` ||
|| **data.FIELDS**
[`object`](../../../../data-types.md) | Object containing information about the fields of the modified template.

The structure is described [below](#fields) ||
|| **ts**
[`timestamp`](../../../../data-types.md) | Date and time the event was sent from the [event queue](../../../../events/index.md) ||
|| **auth**
[`object`](../../../../data-types.md) | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter FIELDS {#fields}

#|
|| **Parameter**
`type` | **Description** ||
|| **ID**
[`integer`](../../../../data-types.md) | Identifier of the record in the regular deal settings table ||
|| **RECURRING_DEAL_ID**
[`integer`](../../../../data-types.md) | Identifier of the regular deal template ||
|#

### Parameter auth {#auth}

{% include notitle [Table with Keys in the auth Array](../../../../../_includes/auth-params-in-events.md) %}

## Continue Exploring

- [{#T}](../../../../events/index.md)
- [{#T}](../../../../events/event-bind.md)
- [{#T}](./on-crm-deal-recurring-delete.md)
- [{#T}](./on-crm-deal-recurring-expose.md)
- [{#T}](./on-crm-deal-recurring-add.md)