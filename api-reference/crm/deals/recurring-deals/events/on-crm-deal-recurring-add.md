# Event When Creating a Recurring Deal Template onCrmDealRecurringAdd

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `ONCRMDEALRECURRINGADD` will trigger when a recurring deal template is created manually or via the method [crm.deal.recurring.add](../crm-deal-recurring-add.md).

Bitrix24 creates the template deal as a regular deal: when a template is created in the interface, or when a regular deal is copied by the `crm.deal.recurring.add` method. Therefore, the [onCrmDealAdd](../../events/on-crm-deal-add.md) event with the `ID` of the template deal arrives before `onCrmDealRecurringAdd` — this value matches `RECURRING_DEAL_ID`. If you pass a template deal with `IS_RECURRING` = `Y` to `crm.deal.recurring.add`, no new deal is created and `onCrmDealAdd` does not arrive.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONCRMDEALRECURRINGADD",
    "event_handler_id": "677",
    "data": {
        "FIELDS": {
            "ID": "11",
            "RECURRING_DEAL_ID": "6797"
        }
    },
    "ts": "1741091781",
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

In this case — `ONCRMDEALRECURRINGADD` ||
|| **event_handler_id**
[`integer`](../../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../../data-types.md) | Object containing information about the created recurrence settings.

Contains a single key `FIELDS` ||
|| **data.FIELDS**
[`object`](../../../../data-types.md) | Object with the identifiers of the created recurrence settings and the template deal.

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
- [{#T}](./on-crm-deal-recurring-update.md)
- [{#T}](./on-crm-deal-recurring-delete.md)
- [{#T}](./on-crm-deal-recurring-expose.md)
