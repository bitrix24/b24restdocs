# How to Add Data to CRM: Overview of Use Cases and Scenarios

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This section helps you select a scenario for CRM integration. The materials describe creating objects and adding related data: files, company details, activities, documents, and SPA configurations.

You can use the tables to select a scenario based on the CRM object, the integration result, and the primary REST methods.

## Select an Object Type

Start by selecting a base object. The choice depends on the customer's interaction stage and the data source.

**Lead**. Suitable for initial inquiries: website applications, chat messages, or cold contacts. If the portal uses classic CRM mode and the inquiry has not yet been qualified, start with a lead.

**Contact and Company**. Use these objects when the customer has already been identified. A contact describes a person, while a company describes an organization. They are often created as a pair.

**Deal**. Required to launch a commercial process. A deal is usually created along with a company and company details if the purpose of the inquiry is a sale.

### Scenarios by Primary Objects

#|
|| **Scenario** | **Main methods** | **Result** ||
|| [Add lead via web form](./how-to-add-lead.md) | [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) | New lead ID ||
|| [Add duplicate lead](./how-to-add-repeat-lead.md) | [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md), [crm.lead.list](../../../api-reference/crm/leads/crm-lead-list.md), [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) | New duplicate lead ID after duplicate check ||
|| [Add contact via web form](./how-to-add-contact.md) | [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md) | New contact ID ||
|| [Add company via web form](./how-to-add-company.md) | [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md) | New company ID ||
|| [Add deal and company with billing details](./how-to-add-deal-with-choice-of-requisite.md) | [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md), [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md), [crm.deal.add](../../../api-reference/crm/deals/crm-deal-add.md) | Company, billing details, and deal ID ||
|#

## Work with Related Data

Company details, addresses, files, and vendors are stored separately from the main CRM card. They are created independently and then linked to a lead, contact, company, or deal.

**Company details and Addresses**. Banking company details and legal addresses are stored separately from contacts and companies. First, retrieve the company details templates, then create the object itself and link an address to it.

**Files**. Files are attached via custom fields. Before creating an object, determine which field the file will be uploaded to.

**Vendors**. A vendor is a separate object type for procurement and warehouse documents. To add a vendor, use universal CRM and catalog methods.

### Scenarios by Related Data

#|
|| **Scenario** | **Main methods** | **Result** ||
|| [Add lead with files via web form](./how-to-add-lead-with-files.md) | [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md), [crm.lead.userfield.add](../../../api-reference/crm/leads/userfield/crm-lead-userfield-add.md) | Lead ID with filled file fields ||
|| [Add contact with billing details via web form](./how-to-add-contact-with-requisite.md) | [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md), [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md), [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md) | Contact, billing details, and address ID ||
|| [Add company with billing details via web form](./how-to-add-company-with-requisite.md) | [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md), [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md), [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md) | Company, billing details, and address ID ||
|| [How to create a vendor in CRM](./how-to-add-contractor.md) | [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md), [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md), [catalog.documentcontractor.add](../../../api-reference/catalog/documentcontractor/catalog-documentcontractor-add.md) | Vendor ID for warehouse documents ||
|#

## Add Activities and Documents

Record actions that should remain in the customer card: meetings, e-mails, tasks, and documents.

**Activities**. Calendar events, e-mails, and tasks are saved in the CRM as activities. When creating them, specify the owner type and the CRM object ID.

**Accounting for CRM Mode**. CRM mode determines where a new inquiry will go: to a lead or directly to a deal. If the integration must work across different portals, check the CRM mode before creating an activity.

**Documents**. Document generation occurs via templates. First, configure the numbering sequence and upload a template, then create a document linked to a CRM object.

### Scenarios by Activities and Documents

#|
|| **Scenario** | **Main methods** | **Result** ||
|| [Add activity to a new lead or deal depending on CRM mode](./how-to-add-objects-with-crm-mode.md) | [crm.settings.mode.get](../../../api-reference/crm/crm-settings-mode-get.md), [crm.deal.list](../../../api-reference/crm/deals/crm-deal-list.md), [crm.activity.todo.add](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-add.md) | Activity ID linked to a lead or deal ||
|| [Add calendar event for client management](./how-to-add-activity-to-contact.md) | [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md), [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) | Activity ID of type "Event" ||
|| [How to send an e-mail to a client on behalf of an employee](./how-to-send-email.md) | [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md), [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) | Activity ID of type "E-mail" ||
|| [How to add a template and create a document based on it](./how-to-generate-documents.md) | [crm.documentgenerator.numerator.add](../../../api-reference/crm/document-generator/numerator/crm-document-generator-numerator-add.md), [crm.documentgenerator.template.add](../../../api-reference/crm/document-generator/templates/crm-document-generator-template-add.md), [crm.documentgenerator.document.add](../../../api-reference/crm/document-generator/documents/crm-document-generator-document-add.md) | Document ID and file link ||
|#

## Configure SPAs

If standard CRM objects are insufficient, use SPAs. These are custom CRM types with their own fields, pipelines, and stages.

**Entity Type ID (entityTypeId)**. A key parameter for working with smart processes. Retrieve it before calling API methods, configuring fields, or adding comments.

**Pipelines and Stages**. Configure the process flow through stages. Create a pipeline and add the necessary statuses to it.

**Custom Fields**. Extend the SPA card with your own fields. Configure the field type, number format, list options, and display parameters.

### Scenarios by Smart Processes

#|
|| **Scenario** | **Main methods** | **Result** ||
|| [How to add a comment to the SPA timeline](./how-to-add-comment-to-spa.md) | [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md), [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md), [crm.timeline.comment.add](../../../api-reference/crm/timeline/comments/crm-timeline-comment-add.md) | Timeline entry ID ||
|| [How to create a custom field in a smart process](./how-to-add-user-field-to-spa.md) | [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md), [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md) | Custom field ID ||
|| [How to configure rounding for a "Number" type custom field](./how-to-add-precision-to-user-field.md) | [crm.userfield.settings.fields](../../../api-reference/crm/universal/user-defined-fields/crm-userfield-settings-fields.md), [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md), [userfieldconfig.update](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-update.md) | Updated number field settings ||
|| [How to create a new pipeline with stages in a smart process](./how-to-add-category-to-spa.md) | [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md), [crm.category.add](../../../api-reference/crm/universal/category/crm-category-add.md), [crm.status.add](../../../api-reference/crm/status/crm-status-add.md) | Pipeline and created stages ID ||
|#
