# Event on changing the deal's Sales Funnel onCrmDealMoveToCategory

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `ONCRMDEALMOVETOCATEGORY` will trigger when the deal's Sales Funnel changes.

Currently, this event can only be subscribed to from the [application](../../../../settings/app-installation/index.md).

## What the handler receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONCRMDEALMOVETOCATEGORY",
    "event_handler_id": "655",
    "data": {
        "FIELDS": {
            "ID": "6675",
            "CATEGORY_ID": "1",
            "STAGE_ID": "C1:NEW"
        }
    },
    "ts": "1736424182",
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
[`string`](../../../data-types.md) | Symbolic code of the event.

In this case — `ONCRMDEALMOVETOCATEGORY`||
|| **event_handler_id**
[`integer`](../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../data-types.md) | Object containing information about the modified deal.

Contains a single key `FIELDS` ||
|| **data.FIELDS**
[`object`](../../../data-types.md) | Object containing information about the fields of the modified deal.

The structure is described [below](#fields) ||
|| **ts**
[`timestamp`](../../../data-types.md) | Date and time of the event sent from the [event queue](../../../events/index.md) ||
|| **auth**
[`object`](../../../data-types.md) | Object containing authorization parameters and data about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter FIELDS {#fields}

#|
|| **Parameter**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the modified deal ||
|| **CATEGORY_ID**
[`integer`](../../../data-types.md) | Identifier of the new Sales Funnel ||
|| **STAGE_ID**
[`string`](../../../data-types.md) | Identifier of the new stage of the deal ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../../_includes/auth-params-in-events.md) %}

## Continue exploring

- [{#T}](../../../events/index.md)
- [{#T}](../../../events/event-bind.md)
- [{#T}](./on-crm-deal-add.md)
- [{#T}](./on-crm-deal-update.md)
- [{#T}](./on-crm-deal-delete.md)