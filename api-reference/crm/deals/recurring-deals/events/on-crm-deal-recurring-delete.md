# Event on Deleting a Recurring Deal Template onCrmDealRecurringDelete

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `ONCRMDEALRECURRINGDELETE` will trigger when a recurring deal template is deleted.

## What the Handler Receives

Data is sent as a POST request {.b24-info}

```json
{
    "event": "ONCRMDEALRECURRINGDELETE",
    "event_handler_id": "681",
    "data": {
        "FIELDS": {
            "ID": "11"
        }
    },
    "ts": "1741092153",
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
[`string`](../../../../data-types.md) | Symbolic event code.

In this case — `ONCRMDEALRECURRINGDELETE`||
|| **event_handler_id**
[`integer`](../../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../../data-types.md) | Object containing information about the deleted template.

Contains a single key `FIELDS` ||
|| **data.FIELDS**
[`object`](../../../../data-types.md) | Object containing information about the fields of the deleted template.

The structure is described [below](#fields) ||
|| **ts**
[`timestamp`](../../../../data-types.md) | Date and time the event was sent from the [event queue](../../../../events/index.md) ||
|| **auth**
[`object`](../../../../data-types.md) | Object containing authorization parameters and data about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter FIELDS {#fields}

#|
|| **Parameter**
`type` | **Description** ||
|| **ID**
[`integer`](../../../../data-types.md) | Identifier of the deleted record in the recurring deals settings table ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../../../events/index.md)
- [{#T}](../../../../events/event-bind.md)
- [{#T}](./on-crm-deal-recurring-add.md)
- [{#T}](./on-crm-deal-recurring-expose.md)
- [{#T}](./on-crm-deal-recurring-update.md)