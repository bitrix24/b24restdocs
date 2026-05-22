# How to pass data to Sales Intelligence

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: a user with permission to create or edit a CRM object

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Sales Intelligence data helps link a lead, deal, contact, company, or estimate to the source of the request and the customer journey. In CRM, you can pass either just the source via UTM fields or a full trace with visit data.

A trace is a set of data about the customer's path before the request: the referral source, pages visited, and other visit parameters. Using the trace, CRM understands where the customer came from and what actions they took before the object was created.

To pass data to Sales Intelligence, choose a method:

#|
|| **If needed** | **What to pass** | **Which methods to rely on** ||
|| Pass only the advertising source when creating an object | UTM fields: `UTM_SOURCE` and others | [CRM object creation methods](../../../api-reference/crm/index.md) ||
|| Pass the full customer journey when creating an object | `TRACE`, if the creation method supports this field | [CRM object creation methods](../../../api-reference/crm/index.md) ||
|| Link one trace to several or to already created objects | `TRACE` and an array of `ENTITIES` objects | [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md) ||
|#

## 1. Pass the UTM source

If the advertising source is sufficient for the report, pass `UTM_SOURCE` when creating a CRM object. The value must match the configured source in Sales Intelligence.

Main CRM objects have UTM fields. Check the list of fields in the description of the method you use to create the object:

- [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) — lead
- [crm.deal.add](../../../api-reference/crm/deals/crm-deal-add.md) — deal
- [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md) — contact
- [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md) — company
- [crm.quote.add](../../../api-reference/crm/quote/crm-quote-add.md) — estimate

This method is suitable when you only need to pass the acquisition channel: advertising system, campaign, ad, or keyword.

The universal method [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) accepts UTM fields in `camelCase`, for example `utmSource`, and saves them in an object. However, it does not form the customer journey in Sales Intelligence: the trace is not created, and the `TRACE` field is not available in the method.

To ensure the data reaches Sales Intelligence, create the object using specific CRM methods or separately link a trace via [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md).

## 2. Pass a full trace when creating an object

A full trace contains data about the customer journey: source, website pages, and other visit parameters. The value for `TRACE` can be retrieved on a website via the Bitrix24 Sales Intelligence JS code:

```js
b24Tracker.guest.getTrace()
```

The Sales Intelligence script must be installed on the website pages where the customer journey is collected. Typically, the `TRACE` value is stored in a hidden form field and sent along with the customer data.

If the object creation method supports the `TRACE` field, pass the retrieved string into it. This option is suitable when an object is created immediately after a form is submitted. For example, a request from a website creates a lead or contact, and Sales Intelligence data is passed along with the main object fields.

For detailed parameters and examples, see the description of the method for creating the required object. Practical scenarios show how to pass `TRACE` during creation:

- [lead](./use-analitics-for-add-lead.md)
- [deal and contact](./use-analitics-for-add-contact.md)

## 3. Link objects with a single trace

If a scenario creates multiple related objects, first create or save the client records, then link them to a single trace using the [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md) method.

For example, a website form might create a contact and a deal. After creating the objects, pass the following to [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md):

- `TRACE` — a string containing Sales Intelligence data
- `ENTITIES` — a list of objects that need to be linked to the trace

This method also works for objects created using the universal [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method: a trace can be linked to them after creation.

The method will return the identifier of the created trace. You can store this identifier on the integration side if the scenario requires deleting the trace or removing the binding later.

## Deleting a trace

Delete a trace if it was erroneously linked to an object or if you need to clear test data.

To delete, use the [crm.tracking.trace.delete](../../../api-reference/crm/tracking/crm-tracking-trace-delete.md) method. Specify the trace identifier `id` returned by the [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md) method.
