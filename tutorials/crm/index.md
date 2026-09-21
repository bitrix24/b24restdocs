# CRM: Common Scenarios

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

This page helps you choose a tutorial for CRM Integration: creating a customer or a deal, updating a card, retrieving lists, passing Sales Intelligence, or embedding an application interface.

Tutorials help you select a working scenario so you do not have to assemble a chain of methods manually. Each scenario specifies access permissions, the request order, identifiers to pass between methods, a code sample, and the result to verify.

The REST API reference groups the same directions on the [{#T}](../../api-reference/crm/tutorials.md) page, where the scenarios are listed alongside the related method groups.

> Quick links: [How to Choose a Direction](#choose-section) and [Common Scenarios](#popular-tutorials)
> 
> User documentation: [How to Start Working with CRM in Bitrix24](https://helpdesk.bitrix24.com/open/25766191/)

## Requirements

**Scope.** All scenarios require the [`crm`](../../api-reference/scopes/permissions.md) scope. Some scenarios may require additional scopes, such as `user` to retrieve user data or `placement` to embed a widget. The exact scopes are specified at the top of each scenario page.

**Permissions.** The user must have permission to perform the required action on the CRM object: read, add, update, or delete. Related data and widgets require the permissions specified in the selected scenario. An inbound webhook operates within its scope, while an OAuth request uses the permissions of the user on whose behalf it is sent.

**Methods.** For new integrations, use the universal `crm.item.*` methods to work with leads, deals, contacts, companies, and SPAs. Development of the object-specific `crm.lead.*` and `crm.deal.*` methods has stopped. Use them only when a specific scenario or API limitation explicitly requires them.

**Responses and Limits.** A successful call returns data in the `result` field, while an error returns the `error` and `error_description` fields. The exact result and error structures for a method are provided on its page. General handling rules are described in [Error Codes](../../error-codes.md), and request rate and execution time limits are described in [REST API Limits](../../limits.md).

## Getting Started

1. Define the integration task: create an object, modify data, retrieve a list, pass analytics, or embed an interface.
2. Select a direction in the [How to Choose a Direction](#choose-section) table.
3. Open the overview for the selected direction and find a scenario matching your task.
4. Verify the access permissions and scopes specified in the scenario.
5. Prepare the CRM object, field, stage, or user identifiers required for the requests.
6. Execute the methods in the order described in the scenario.

## How to Choose a Direction {#choose-section}

#|
|| **If needed** | **Open** | **What you will find** ||
|| Create a lead, contact, company, deal, activity, document, or SPA item | [Add data](./how-to-add-crm-objects/index.md) | Scenarios for creating CRM objects and related data. Main methods and groups: `crm.item.add`, `crm.activity.*`, `crm.requisite.*` ||
|| Change card fields, phones, email, activity date, activity link, or payment date | [Edit data](./how-to-edit-crm-objects/index.md) | Scenarios for updating CRM data. Main methods and groups: `crm.item.update`, `crm.activity.binding.*`, `crm.item.payment.list`, `crm.deal.userfield.*` ||
|| Find duplicates, get activities, stages, pipelines, addresses, suppliers, or items by filter | [Get lists](./how-to-get-lists/index.md) | Scenarios for retrieving data from CRM. Main methods and groups: `crm.duplicate.findbycomm`, `crm.*.list`, `crm.activity.list`, `crm.status.*`, `crm.category.*`, `crm.item.list`, `crm.requisite.*` ||
|| Pass UTM tags, `TRACE`, or link created objects to a trace | [Sales Intelligence](./how-to-use-analitycs/index.md) | Scenarios for passing analytics data. Main methods: `crm.item.add`, `crm.tracking.trace.add` ||
|| Add the application interface to a CRM card | [How to Embed Widgets into CRM](./crm-widgets/index.md) | Scenarios for a lead custom field and a CRM card tab. Main methods and embedding points: `userfieldtype.add`, `placement.bind`, CRM ||
|#

## Relationships with Other Objects

Scenarios work with core CRM objects and related data.

**Leads, Contacts, Companies, and Deals.** Create, update, and retrieve core customer and sales cards using the universal [crm.item.*](../../api-reference/crm/universal/index.md) methods. The documentation also includes the object-specific [crm.lead.*](../../api-reference/crm/leads/index.md) and [crm.deal.*](../../api-reference/crm/deals/index.md) groups for leads and deals, but their development has stopped.

**Activities and Timeline.** Link calls, emails, meetings, tasks, and comments to CRM cards using [activity](../../api-reference/crm/timeline/activities/index.md) and [timeline comment](../../api-reference/crm/timeline/comments/index.md) methods.

**Company Details, Addresses, and Line Items.** Related data is stored separately from the CRM card. Create it and link it to a customer, deal, or document using [company details](../../api-reference/crm/requisites/index.md), [addresses](../../api-reference/crm/requisites/addresses/index.md), [line items](../../api-reference/crm/universal/product-rows/index.md), and [catalog](../../api-reference/catalog/index.md) methods.

**SPAs.** Custom CRM types use `entityTypeId`, the object type ID. Use the universal [crm.item.*](../../api-reference/crm/universal/index.md) methods to create items, pipelines, stages, and custom fields.

**Sales Intelligence.** Pass the inquiry source to the universal `crm.item.add` method using UTM fields. Link the complete customer journey to the created object using [crm.tracking.trace.add](../../api-reference/crm/tracking/crm-tracking-trace-add.md), because `crm.item.add` does not accept the `TRACE` field.

**CRM Widgets.** Embed an application interface into a CRM card using a custom field or tab. To register handlers, use the [widgets](../../api-reference/widgets/index.md) methods and embedding locations.

## Common Scenarios {#popular-tutorials}

The table below is a selection of tasks used to begin working with the CRM. For a full list of materials, see the direction overviews: [add data](./how-to-add-crm-objects/index.md), [edit data](./how-to-edit-crm-objects/index.md), [retrieve lists](./how-to-get-lists/index.md), [Sales Intelligence](./how-to-use-analitycs/index.md), and [CRM widgets](./crm-widgets/index.md).

#|
|| **If needed** | **Open** ||
|| Add a lead from a website form | [How to add a lead](./how-to-add-crm-objects/how-to-add-lead.md) ||
|| Add a lead with files | [How to add a lead with files](./how-to-add-crm-objects/how-to-add-lead-with-files.md) ||
|| Add a contact or company with requisites | [How to add a contact with requisites](./how-to-add-crm-objects/how-to-add-contact-with-requisite.md) or [how to add a company with requisites](./how-to-add-crm-objects/how-to-add-company-with-requisite.md) ||
|| Add a deal and select the company's Company details | [How to add a deal and a company with requisites](./how-to-add-crm-objects/how-to-add-deal-with-choice-of-requisite.md) ||
|| Create an activity in a lead or deal, taking the CRM mode into account | [How to add an activity to a new lead or deal depending on the CRM mode](./how-to-add-crm-objects/how-to-add-objects-with-crm-mode.md) ||
|| Change the scheduled activity date | [How to change the time of a scheduled activity](./how-to-edit-crm-objects/how-to-change-date-in-activity.md) ||
|| Change client phone or email | [How to change or delete phone numbers and email](./how-to-edit-crm-objects/how-to-change-email-or-phone.md) ||
|| Move an activity between CRM cards | [How to move an activity between items of the same type](./how-to-edit-crm-objects/how-to-move-activity.md) or [how to move an activity from one object type to another](./how-to-edit-crm-objects/how-to-move-activity-between-objects.md) ||
|| Find duplicates by phone or email | [How to find duplicates in CRM by phone and email](./how-to-get-lists/search-by-phone-and-email.md) ||
|| Get stages, pipelines, or items by stage | [How to get a list of stages with semantics](./how-to-get-lists/how-to-get-stages-with-semantics.md), [how to get deal pipelines](./how-to-get-lists/how-to-get-deal-funnels.md), or [how to filter items by stage name](./how-to-get-lists/how-to-get-elements-by-stage-filter.md) ||
|| Pass Sales Intelligence data when creating a lead or a deal | [How to use Sales Intelligence when creating a lead](./how-to-use-analitycs/use-analitics-for-add-lead.md) or [how to use Sales Intelligence when creating a deal and a contact](./how-to-use-analitycs/use-analitics-for-add-contact.md) ||
|| Embed an application interface into a CRM card | [How to embed a widget into a lead as a custom field](./crm-widgets/widget-as-field-in-lead-page.md) or [how to embed a widget into a CRM card tab](./crm-widgets/widget-as-detail-tab.md) ||
|#
