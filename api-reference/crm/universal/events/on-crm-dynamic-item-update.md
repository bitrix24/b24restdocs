# Event for Updating a Custom Type CRM Object onCrmDynamicItemUpdate

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: `any user`

The event is triggered when an element of any [custom object type](../user-defined-object-types/index.md) in CRM is modified.

## What the Handler Receives

Data is sent as a POST request {.b24-info}

Event for changing an element with `id = 24`, belonging to a SPA with `entityTypeId = 1220`:

```json
{
    "event": "ONCRMDYNAMICITEMUPDATE",
    "event_handler_id": "5",
    "data": {
        "FIELDS": {
            "ID": "24",
            "ENTITY_TYPE_ID": "1220"
        }
    },
    "ts": "1723538517",
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": "3600",
        "scope": "crm",
        "domain": "some-domain.bitrix24.com",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "status": "F",
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
[`string`][1] | Symbolic code of the event.

In this case, `ONCRMDYNAMICITEMUPDATE`||
|| **event_handler_id**
[`integer`][1] | Identifier of the event handler ||
|| **data**
[`object`][1] | Object containing information about the modified custom type CRM object.

Contains a single key `FIELDS` ||
|| **data.FIELDS**
[`object`][1] | Object containing information about the fields of the modified custom type CRM object.

The structure is described [below](#fields) ||
|| **ts**
[`timestamp`][1] | Date and time the event was sent from the [event queue](../../../events/index.md) ||
|| **auth**
[`object`][1] | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### FIELDS Parameter {#fields}

#|
|| **Parameter**
`type` | **Description** ||
|| **ID**
[`integer`][1] | Identifier of the modified custom type CRM object ||
|| **ENTITY_TYPE_ID**
[`integer`][1] | Identifier of the custom type CRM ||
|#

### auth Parameter {#auth}

{% include notitle [Table with keys in the auth array](../../../../_includes/auth-params-in-events.md) %}

{% note warning "System Object Type Events" %}

Although [universal CRM methods](../index.md) allow adding and modifying objects of standard types such as deals, leads, contacts, companies, and estimates, the event `onCrmDynamicItemUpdate` will not trigger when modifying the listed objects.

To track changes in deals, leads, etc., you can use specific events:

- [{#T}](../../deals/events/on-crm-deal-update.md)
- [{#T}](../../leads/events/on-crm-lead-update.md)
- [{#T}](../../contacts/events/on-crm-contact-update.md)
- [{#T}](../../companies/events/on-crm-company-update.md)
- [{#T}](../../quote/events/on-crm-quote-update.md)

When new invoices are modified, the event `onCrmDynamicItemUpdate` will trigger.

{% endnote %}

## Continue Learning

- [{#T}](../../../events/index.md)
- [{#T}](../../../events/event-bind.md)
- [{#T}](index.md)
- [{#T}](on-crm-dynamic-item-add.md)
- [{#T}](on-crm-dynamic-item-delete.md)

[1]: ../../../data-types.md