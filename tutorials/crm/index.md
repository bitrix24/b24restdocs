# CRM: Common Scenarios

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

CRM Integration begins with an application task: creating a customer or a deal, updating a card, retrieving lists, passing Sales Intelligence, or embedding an application interface.

Tutorials help you select a working scenario so you do not have to assemble a chain of methods manually. Each scenario specifies access permissions, the request order, identifiers to be passed between methods, a code sample, and the result to be verified.

> Quick links: [How to Choose a Direction](#choose-section) and [Common Scenarios](#popular-tutorials)
> 
> User documentation: [CRM overview](https://helpdesk.bitrix24.com/open/25766191/)

## Connection with CRM Objects

Scenarios work with core CRM objects and related data.

**Leads, contacts, companies, and deals.** Core customer and sales cards can be created, updated, and retrieved using the [crm.lead.*](../../api-reference/crm/leads/index.md), [crm.contact.*](../../api-reference/crm/contacts/index.md), [crm.company.*](../../api-reference/crm/companies/index.md), and [crm.deal.*](../../api-reference/crm/deals/index.md) methods.

**Activities and Timeline.** Calls, emails, meetings, tasks, and comments are linked to CRM cards via [activity](../../api-reference/crm/timeline/activities/index.md) and [timeline comment](../../api-reference/crm/timeline/comments/index.md) methods.

**Company details, addresses, and line items.** Related data is stored separately from the CRM card. These are created and linked to a customer, deal, or document using [company details](../../api-reference/crm/requisites/index.md), [addresses](../../api-reference/crm/requisites/addresses/index.md), [line items](../../api-reference/crm/universal/product-rows/index.md), and [catalog](../../api-reference/catalog/index.md) methods.

**SPAs.** Custom CRM types operate via `entityTypeId` — an object type identifier. To create items, pipelines, stages, and custom fields, use the universal [crm.item.*](../../api-reference/crm/universal/index.md) methods.

**Sales Intelligence.** The source of inquiry and the customer route are passed to the CRM via UTM parameters, the `TRACE` field, or the [crm.tracking.trace.add](../../api-reference/crm/tracking/crm-tracking-trace-add.md) method.

**CRM Widgets.** An application interface can be embedded into a CRM card via a custom field or a tab. To register handlers, use the [widgets](../../api-reference/widgets/index.md) methods and embedding points.

## Getting Started

1. Define the integration task: create an object, modify data, retrieve a list, pass analytics, or embed an interface.
2. Select a direction in the [How to Choose a Direction](#choose-section) table.
3. Open the overview for the selected direction and find a scenario for your task.
4. Verify the access permissions and scopes specified in the scenario.
5. Prepare the CRM object, field, stage, or user identifiers required for the requests.
6. Execute the methods in the order described in the scenario.

## How to Choose a Direction {#choose-section}

#|
|| **If needed** | **Open** | **What you will find** ||
|| Create a lead, contact, company, deal, activity, document, or SPA item | [Add data](./how-to-add-crm-objects/index.md) | 19 scenarios for creating CRM objects and related data. Main methods and groups: `crm.lead.add`, `crm.contact.add`, `crm.company.add`, `crm.deal.add`, `crm.item.add`, `crm.activity.*`, `crm.requisite.*` ||
|| Change card fields, phones, email, activity date, activity link, or payment date | [Edit data](./how-to-edit-crm-objects/index.md) | 10 scenarios for updating CRM data. Main methods and groups: `crm.lead.update`, `crm.contact.update`, `crm.company.update`, `crm.deal.update`, `crm.activity.binding.*`, `crm.item.payment.list`, `crm.deal.userfield.*` ||
|| Find duplicates, get activities, stages, pipelines, addresses, suppliers, or items by filter | [Get lists](./how-to-get-lists/index.md) | 7 scenarios for retrieving data from CRM. Main methods and groups: `crm.duplicate.findbycomm`, `crm.*.list`, `crm.activity.list`, `crm.status.*`, `crm.category.*`, `crm.item.list`, `crm.requisite.*` ||
|| Pass UTM tags, `TRACE`, or link created objects to a trace | [Sales Intelligence](./how-to-use-analitycs/index.md) | 3 scenarios for passing analytics data. Main methods: `crm.lead.add`, `crm.contact.add`, `crm.company.add`, `crm.deal.add`, `crm.item.add`, `crm.tracking.trace.add` ||
|| Add the application interface to a CRM card | [How to embed widgets into CRM](./crm-widgets/index.md) | 2 scenarios for a lead custom field and a CRM card tab. Main methods and embedding points: `userfieldtype.add`, `placement.bind`, CRM ||
|#

## Common Scenarios {#popular-tutorials}

The table below is a selection of tasks used to begin working with the CRM. For a full list of materials, see the direction overviews: [add data](./how-to-add-crm-objects/index.md), [edit data](./how-to-edit-crm-objects/index.md), [retrieve lists](./how-to-get-lists/index.md), [Sales Intelligence](./how-to-use-analitycs/index.md), and [CRM widgets](./crm-widgets/index.md).

#|
|| **If needed** | **Open** ||
|| Add a lead from a website form | [How to add a lead](./how-to-add-crm-objects/how-to-add-lead.md) ||
|| Add a lead with files | [How to add a lead with files](./how-to-add-crm-objects/how-to-add-lead-with-files.md) ||
|| Add a contact or company with billing details | [How to add a contact with billing details](./how-to-add-crm-objects/how-to-add-contact-with-requisite.md) or [how to add a company with billing details](./how-to-add-crm-objects/how-to-add-company-with-requisite.md) ||
|| Add a deal and select the company's Company details | [How to add a deal and a company with billing details](./how-to-add-crm-objects/how-to-add-deal-with-choice-of-requisite.md) ||
|| Create an activity in a lead or deal, taking the CRM mode into account | [How to add an activity to a new lead or deal depending on the CRM mode](./how-to-add-crm-objects/how-to-add-objects-with-crm-mode.md) ||
|| Change the scheduled activity date | [How to change the time of a scheduled activity](./how-to-edit-crm-objects/how-to-change-date-in-activity.md) ||
|| Change client phone or email | [How to change or delete phone numbers and email](./how-to-edit-crm-objects/how-to-change-email-or-phone.md) ||
|| Move activity between CRM cards | [How to move activity between items of the same type](./how-to-edit-crm-objects/how-to-move-activity.md) or [how to move activity from one object type to another](./how-to-edit-crm-objects/how-to-move-activity-between-objects.md) ||
|| Find duplicates by phone or email | [How to find duplicates in CRM by phone and email](./how-to-get-lists/search-by-phone-and-email.md) ||
|| Get stages, pipelines, or items by stage | [How to get a list of stages with semantics](./how-to-get-lists/how-to-get-stages-with-semantics.md), [how to get deal pipelines](./how-to-get-lists/how-to-get-deal-funnels.md), or [how to filter items by stage name](./how-to-get-lists/how-to-get-elements-by-stage-filter.md) ||
|| Pass Sales Intelligence data when creating a lead or a deal | [How to use Sales Intelligence when creating a lead](./how-to-use-analitycs/use-analitics-for-add-lead.md) or [how to use Sales Intelligence when creating a deal and a contact](./how-to-use-analitycs/use-analitics-for-add-contact.md) ||
|| Embed the application interface into a CRM card | [How to embed a widget into a lead as a custom field](./crm-widgets/widget-as-field-in-lead-page.md) or [how to embed a widget into a CRM card tab](./crm-widgets/widget-as-detail-tab.md) ||
|#
