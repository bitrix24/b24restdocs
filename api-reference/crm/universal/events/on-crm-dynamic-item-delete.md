# Event for Deleting a Custom Type CRM Object onCrmDynamicItemDelete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: `any user`

The event will trigger upon the deletion of an item of any [custom object type](../user-defined-object-types/index.md) in CRM.

## What the handler receives

Data is sent as a POST request {.b24-info}

The event for deleting an item with the identifier `23`, belonging to the SPA with `entityTypeId = 1220`:

```json
{
    "event": "ONCRMDYNAMICITEMDELETE",
    "event_handler_id": "6",
    "data": {
        "FIELDS": {
            "ID": "23",
            "ENTITY_TYPE_ID": "1220"
        }
    },
    "ts": "1723538149",
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

In this case — `ONCRMDYNAMICITEMDELETE`||
|| **event_handler_id**
[`integer`][1] | Identifier of the event handler ||
|| **data**
[`object`][1] | Object containing information about the deleted custom type CRM object.

Contains a single key `FIELDS` ||
|| **data.FIELDS**
[`object`][1] | Object containing information about the fields of the deleted custom type CRM object.

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
[`integer`][1] | Identifier of the deleted custom type CRM object ||
|| **ENTITY_TYPE_ID**
[`integer`][1] | Identifier of the custom type CRM ||
|#

### auth Parameter {#auth}

{% include notitle [Table with keys in the auth array](../../../../_includes/auth-params-in-events.md) %}

{% note warning "System Object Type Events" %}

Although [universal CRM methods](../index.md) allow adding and modifying objects of standard types such as deals, leads, contacts, companies, and estimates, the event `onCrmDynamicItemDelete` will not trigger when deleting the aforementioned objects.

To track deleted deals, leads, etc., you can use specific events:

- [{#T}](../../deals/events/on-crm-deal-delete.md)
- [{#T}](../../leads/events/on-crm-lead-delete.md)
- [{#T}](../../contacts/events/on-crm-contact-delete.md)
- [{#T}](../../companies/events/on-crm-company-delete.md)
- [{#T}](../../quote/events/on-crm-quote-delete.md)

When deleting new invoices, the event `onCrmDynamicItemDelete` will trigger.

{% endnote %}

## Continue Learning

- [{#T}](../../../events/index.md)
- [{#T}](../../../events/event-bind.md)
- [{#T}](index.md)
- [{#T}](on-crm-dynamic-item-add.md)
- [{#T}](on-crm-dynamic-item-update.md)

[1]: ../../../data-types.md