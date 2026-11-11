# Event on Changing Recurrence Settings of a Recurring Deal onCrmDealRecurringUpdate

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `ONCRMDEALRECURRINGUPDATE` will trigger in three cases:

- when the recurrence settings are modified manually or via the method [crm.deal.recurring.update](../crm-deal-recurring-update.md)
- when a deal is created from the template, right after [onCrmDealRecurringExpose](./on-crm-deal-recurring-expose.md)
- when a scheduled deal could not be created — Bitrix24 disables the recurrence, and the event arrives without `onCrmDealRecurringExpose`

After a deal is created from the template, Bitrix24 writes the repeat counter and the last run date to the settings. On a scheduled run, it also recalculates the next run date.

The event is not triggered when fields of the template deal itself are modified, such as its title or amount. Such changes are tracked by the [onCrmDealUpdate](../../events/on-crm-deal-update.md) event.

The event does not contain a list of changed settings — request the current values by the `ID` from the [FIELDS parameter](#fields).

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

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
        "application_token": "51856fefc120afa4b628cc82d3935cce"
    }
}
```

#|
|| **parameter**
`type` | **Description** ||
|| **event**
[`string`](../../../../data-types.md) | Symbolic code of the event.

In this case — `ONCRMDEALRECURRINGUPDATE` ||
|| **event_handler_id**
[`integer`](../../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../../data-types.md) | Object containing information about the modified recurrence settings.

Contains a single key `FIELDS` ||
|| **data.FIELDS**
[`object`](../../../../data-types.md) | Object with the identifiers of the modified recurrence settings and the template deal.

The structure is described [below](#fields) ||
|| **ts**
[`timestamp`](../../../../data-types.md) | Date and time of the event sent from the [event queue](../../../../events/index.md) ||
|| **auth**
[`object`](../../../../data-types.md) | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter FIELDS {#fields}

#|
|| **parameter**
`type` | **Description** ||
|| **ID**
[`integer`](../../../../data-types.md) | Identifier of the recurrence settings. Pass it to the `id` parameter of [crm.deal.recurring.get](../crm-deal-recurring-get.md) to retrieve the settings ||
|| **RECURRING_DEAL_ID**
[`integer`](../../../../data-types.md) | Identifier of the template deal whose field values are copied. Matches the `DEAL_ID` field in the [crm.deal.recurring.get](../crm-deal-recurring-get.md) response. Fields of the template deal are returned by [crm.deal.get](../../crm-deal-get.md) ||
|#

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../../../events/index.md)
- [{#T}](../../../../events/event-bind.md)
- [{#T}](./on-crm-deal-recurring-add.md)
- [{#T}](./on-crm-deal-recurring-delete.md)
- [{#T}](./on-crm-deal-recurring-expose.md)
