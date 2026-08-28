# Typical use-cases and scenarios for REST API in CRM and tutorials

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The CRM reference describes individual methods, while tutorials show how to assemble a working scenario out of them: in which order to call the methods, which identifiers to pass between requests, and what to verify in the result.

Tutorials are grouped into five directions. Select a direction by your task, and inside it a tutorial for the specific case. An overview of the directions with a table of common scenarios is collected on the [{#T}](../../tutorials/crm/index.md) page.

> Quick links: [All Scenarios](#choose-tutorial)
>
> User documentation: [How to Start Working with CRM in Bitrix24](https://helpdesk.bitrix24.com/open/25766191/)

## Scenarios by Direction {#choose-tutorial}

### Add Data

The scenarios create leads, contacts, companies, deals, activities, documents, and smart process items, and also link requisites, addresses, and product rows to them. Main method groups: [crm.lead.*](./leads/index.md), [crm.contact.*](./contacts/index.md), [crm.company.*](./companies/index.md), [crm.deal.*](./deals/index.md), [crm.item.*](./universal/index.md), [activities](./timeline/activities/index.md), and [requisites](./requisites/index.md). An overview of the direction is available in [{#T}](../../tutorials/crm/how-to-add-crm-objects/index.md).

#|
|| **If You Need To** | **Open** ||
|| Accept a request from a website form and create a lead | [{#T}](../../tutorials/crm/how-to-add-crm-objects/how-to-add-lead.md) ||
|| Create a lead for a customer who has already contacted you | [{#T}](../../tutorials/crm/how-to-add-crm-objects/how-to-add-repeat-lead.md) ||
|| Create a contact from website form data | [{#T}](../../tutorials/crm/how-to-add-crm-objects/how-to-add-contact.md) ||
|| Create a company from website form data | [{#T}](../../tutorials/crm/how-to-add-crm-objects/how-to-add-company.md) ||
|| Create a deal and a company with a choice of requisites | [{#T}](../../tutorials/crm/how-to-add-crm-objects/how-to-add-deal-with-choice-of-requisite.md) ||
|| Accept a request with attached files | [{#T}](../../tutorials/crm/how-to-add-crm-objects/how-to-add-lead-with-files.md) ||
|| Create a contact together with requisites and an address | [{#T}](../../tutorials/crm/how-to-add-crm-objects/how-to-add-contact-with-requisite.md) ||
|| Create a company together with requisites and an address | [{#T}](../../tutorials/crm/how-to-add-crm-objects/how-to-add-company-with-requisite.md) ||
|| Add a vendor for inventory documents | [{#T}](../../tutorials/crm/how-to-add-crm-objects/how-to-add-contractor.md) ||
|| Create an activity taking the CRM mode into account — simple or classic | [{#T}](../../tutorials/crm/how-to-add-crm-objects/how-to-add-objects-with-crm-mode.md) ||
|| Schedule a meeting or a calendar event for a customer | [{#T}](../../tutorials/crm/how-to-add-crm-objects/how-to-add-activity-to-contact.md) ||
|| Send an email to a customer on behalf of an employee | [{#T}](../../tutorials/crm/how-to-add-crm-objects/how-to-send-email.md) ||
|| Build a document from a template — an invoice, a contract, or a statement | [{#T}](../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md) ||
|| Post a comment to the timeline of a smart process | [{#T}](../../tutorials/crm/how-to-add-crm-objects/how-to-add-comment-to-spa.md) ||
|| Add a custom field to a smart process | [{#T}](../../tutorials/crm/how-to-add-crm-objects/how-to-add-user-field-to-spa.md) ||
|| Set the rounding of a numeric field | [{#T}](../../tutorials/crm/how-to-add-crm-objects/how-to-add-precision-to-user-field.md) ||
|| Create a pipeline with stages in a smart process | [{#T}](../../tutorials/crm/how-to-add-crm-objects/how-to-add-category-to-spa.md) ||
|| Add products with discounts and taxes to a CRM object | [{#T}](../../tutorials/crm/how-to-add-crm-objects/how-to-product-binding.md) ||
|#

### Edit Data

The scenarios update card fields, phone numbers and emails, move activities between objects, and write the payment date into a deal field. Main method groups: [crm.item.*](./universal/index.md), [crm.lead.*](./leads/index.md), [crm.contact.*](./contacts/index.md), [crm.company.*](./companies/index.md), [crm.deal.*](./deals/index.md), and [activities](./timeline/activities/index.md). An overview of the direction is available in [{#T}](../../tutorials/crm/how-to-edit-crm-objects/index.md).

#|
|| **If You Need To** | **Open** ||
|| Provide an employee with a web form for creating and editing a lead | [{#T}](../../tutorials/crm/how-to-edit-crm-objects/how-to-generate-edit-form-for-lead.md) ||
|| Provide a web form for creating and editing a contact | [{#T}](../../tutorials/crm/how-to-edit-crm-objects/how-to-make-contact-edit-card.md) ||
|| Provide a web form for creating and editing a company | [{#T}](../../tutorials/crm/how-to-edit-crm-objects/how-to-generate-edit-form-for-company.md) ||
|| Provide a web form for a deal with a choice of pipeline and stage | [{#T}](../../tutorials/crm/how-to-edit-crm-objects/how-to-generate-edit-form-for-deal.md) ||
|| Change or delete a customer phone number and email | [{#T}](../../tutorials/crm/how-to-edit-crm-objects/how-to-change-email-or-phone.md) ||
|| Shift the due date and reminders of a scheduled activity | [{#T}](../../tutorials/crm/how-to-edit-crm-objects/how-to-change-date-in-activity.md) ||
|| Move an activity to another item of the same type | [{#T}](../../tutorials/crm/how-to-edit-crm-objects/how-to-move-activity.md) ||
|| Move an activity to an object of a different type | [{#T}](../../tutorials/crm/how-to-edit-crm-objects/how-to-move-activity-between-objects.md) ||
|| Copy the payment date into a deal field | [{#T}](../../tutorials/crm/how-to-edit-crm-objects/how-to-set-paid-date-to-deal.md) ||
|#

### Retrieve Lists

The scenarios find duplicates, retrieve activities, stages, pipelines, addresses, and vendors, and filter items by stage. Main method groups: [crm.duplicate.*](./duplicates/index.md), [crm.status.*](./status/index.md), [crm.category.*](./universal/category/index.md), [crm.item.*](./universal/index.md), and [requisites](./requisites/index.md). An overview of the direction is available in [{#T}](../../tutorials/crm/how-to-get-lists/index.md).

#|
|| **If You Need To** | **Open** ||
|| Check whether the customer is already in CRM before creating a new one | [{#T}](../../tutorials/crm/how-to-get-lists/search-by-phone-and-email.md) ||
|| Retrieve a customer address from requisites | [{#T}](../../tutorials/crm/how-to-get-lists/how-to-get-address.md) ||
|| Retrieve the stages of an object together with their semantics | [{#T}](../../tutorials/crm/how-to-get-lists/how-to-get-stages-with-semantics.md) ||
|| Retrieve deal pipelines with their stages | [{#T}](../../tutorials/crm/how-to-get-lists/how-to-get-deal-funnels.md) ||
|| Select the items that are at a particular stage | [{#T}](../../tutorials/crm/how-to-get-lists/how-to-get-elements-by-stage-filter.md) ||
|| Retrieve activities linked to an employee’s deals | [{#T}](../../tutorials/crm/how-to-get-lists/get-activity-list-by-deals.md) ||
|| Select contacts and companies marked as vendors | [{#T}](../../tutorials/crm/how-to-get-lists/how-to-get-contractors.md) ||
|#

### Sales Intelligence

The scenarios pass the source of the inquiry and the customer journey into CRM through UTM fields, the `TRACE` field, or a separate trace. Main method groups: [CRM tracking](./tracking/index.md) and the methods that create CRM objects. An overview of the direction is available in [{#T}](../../tutorials/crm/how-to-use-analitycs/index.md).

#|
|| **If You Need To** | **Open** ||
|| Choose how to pass the data — UTM fields, `TRACE`, or a separate trace | [{#T}](../../tutorials/crm/how-to-use-analitycs/info-to-analitics.md) ||
|| Pass the source of the inquiry when creating a lead | [{#T}](../../tutorials/crm/how-to-use-analitycs/use-analitics-for-add-lead.md) ||
|| Link a contact and a deal to a single trace | [{#T}](../../tutorials/crm/how-to-use-analitycs/use-analitics-for-add-contact.md) ||
|#

### Widgets in CRM

The scenarios embed the application interface into a CRM card — through a custom field or a separate tab. Main method groups: [widgets](../widgets/index.md) and [custom field types](./universal/user-defined-fields/index.md). An overview of the direction is available in [{#T}](../../tutorials/crm/crm-widgets/index.md).

#|
|| **If You Need To** | **Open** ||
|| Show the application inside a field of a lead card | [{#T}](../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md) ||
|| Add your own tab to a CRM card | [{#T}](../../tutorials/crm/crm-widgets/widget-as-detail-tab.md) ||
|#

## Continue Your Exploration

- [{#T}](../../tutorials/index.md)
- [{#T}](./index.md)