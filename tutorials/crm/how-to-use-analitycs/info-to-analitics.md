# How to Transfer Information to Sales Intelligence

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- The method crm.tracking.trace.add is mentioned on this page, but it is not documented anywhere.

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

When creating CRM entities, there are three ways to transfer information for Sales Intelligence.

## The Simplest Method

Transfer the `UTM_SOURCE` field in the fields of the created entity.

In this case, when the entity is created, if a configured source in Sales Intelligence with the same `UTM_SOURCE` is found, this source will be assigned to the entity, a corresponding icon will be displayed, and the entity will participate in the Sales Intelligence report.

## Complete Data

Transfer the `TRACE` field in the fields of the created entity.

In this case, all data will be taken into account — device, all channels, including the website, and visited pages.

This method works for the following methods: [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md), [crm.deal.add](../../../api-reference/crm/deals/crm-deal-add.md), [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md), [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md), [crm.quote.add](../../../api-reference/crm/quote/crm-quote-add.md).

```json
{
    "fields": {
        "NAME": "test",
        "LAST_NAME": "",
        "TRACE": ...
    }
}
```

The value of the `TRACE` field must be either the identifier of the saved Sales Intelligence record or a JSON string with an array of a specific format, which can be easily obtained using the JS code of the Bitrix24 Sales Intelligence widget:

```js
b24Tracker.guest.getTrace()
```

The value of the `TRACE` field can be a number — the `ID` of the trace obtained by the method `crm.tracking.trace.add`.

## Creating a Trace and Obtaining Its ID

The method creates a trace:

```bash
crm.tracking.trace.add
?ENTITIES[0][TYPE]=CONTACT&ENTITIES[0][ID]=3215&ENTITIES[1][TYPE]=LEAD&ENTITIES[1][ID]=1&TRACE=
```

The `TRACE` field is required, and the value is a string obtained from the method `b24Tracker.guest.getTrace`. See the example above.

The `ENTITIES` field is optional; it can list the entities associated with this trace:

```js
ENTITIES: [
    {
        TYPE: 'CONTACT',
        ID: 1
    },
    {
        TYPE: 'LEAD',
        ID: 101
    }
]
```

## One Trace for Related Entities

If a package of related entities (deal + contact + company) is being created, a single trace can be created for them. If the contact and company already exist, and only the deal is being created, a trace can be created and linked to the existing entities.

## Continue Your Exploration

- [Using Sales Intelligence When Creating a Lead](./use-analitics-for-add-lead.md)
- [Using Sales Intelligence When Creating a Deal and Contact](./use-analitics-for-add-contact.md)