# How to Add Data to CRM: Overview of Use-Cases and Scenarios

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This section helps you choose a scenario for integration with CRM. The materials describe the creation of objects and the addition of related data: files, requisites, CRM activities, documents, and settings for Smart Process Automation (SPA).

You can select a scenario based on the CRM entity, the integration result, and the main REST methods.

## Choose the Type of Object

Start by selecting the base object. The choice depends on the stage of interaction with the client and the source of the data.

**Lead**. Suitable for initial inquiries: requests from the website, messages from chats, or cold contacts. If the account uses the classic CRM mode and the inquiry has not yet been qualified, start with a lead.

**Contact and Company**. Use these objects when the client has already been identified. A contact describes a person, while a company describes an organization. They are often created together.

**Deal**. Necessary to initiate a commercial process. A deal is usually created along with a company and requisites if the purpose of the inquiry is a sale.

### Scenarios for Main Objects

#|
|| **Scenario** | **Main Methods** | **Result** ||
|| [Add Lead via Web Form](./how-to-add-lead.md) | [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) | ID of the new lead ||
|| [Add Duplicate Lead](./how-to-add-repeat-lead.md) | [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md), [crm.lead.list](../../../api-reference/crm/leads/crm-lead-list.md), [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) | ID of the new duplicate lead after checking for duplicates ||
|| [Add Contact via Web Form](./how-to-add-contact.md) | [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md) | ID of the new contact ||
|| [Add Company via Web Form](./how-to-add-company.md) | [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md) | ID of the new company ||
|| [Add Deal and Company with Requisites](./how-to-add-deal-with-choice-of-requisite.md) | [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md), [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md), [crm.deal.add](../../../api-reference/crm/deals/crm-deal-add.md) | ID of the company, requisite, and deal ||
|#

## Work with Related Data

Requisites, addresses, files, and vendors are stored separately from the main CRM entity. They are created independently and then linked to a lead, contact, company, or deal.

**Requisites and Addresses**. Bank requisites and legal addresses are stored separately from contacts and companies. First, obtain the requisite templates, then create the object and link the address to it.

**Files**. Files are attached through custom fields. Before creating the object, determine which field the file will be uploaded to.

**Vendors**. A vendor is a separate type of object for procurement and warehouse documents. Use universal CRM and catalog methods to add a vendor.

### Scenarios for Related Data

#|
|| **Scenario** | **Main Methods** | **Result** ||
|| [Add Lead with Files via Web Form](./how-to-add-lead-with-files.md) | [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md), [crm.lead.userfield.add](../../../api-reference/crm/leads/userfield/crm-lead-userfield-add.md) | ID of the lead with populated file fields ||
|| [Add Contact with Requisites via Web Form](./how-to-add-contact-with-requisite.md) | [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md), [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md), [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md) | ID of the contact, requisite, and address ||
|| [Add Company with Requisites via Web Form](./how-to-add-company-with-requisite.md) | [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md), [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md), [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md) | ID of the company, requisite, and address ||
|| [How to Create a Vendor in CRM](./how-to-add-contractor.md) | [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md), [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md), [catalog.documentcontractor.add](../../../api-reference/catalog/documentcontractor/catalog-documentcontractor-add.md) | ID of the vendor for warehouse documents ||
|#

## Add Activities and Documents

Record actions that should remain in the client's card: meetings, e-mails, tasks, and documents.

**CRM Activities**. Calendar events, e-mails, and tasks are saved in CRM as activities. When creating, specify the owner type and the ID of the CRM entity.

**CRM Mode Consideration**. The CRM mode determines where a new inquiry will go: to a lead or directly to a deal. If the integration needs to work across different accounts, check the CRM mode before creating an activity.

**Documents**. Document generation occurs based on templates. First, set up the numbering and upload the template, then create the document linked to the CRM entity.

### Scenarios for Activities and Documents

#|
|| **Scenario** | **Main Methods** | **Result** ||
|| [Add Activity to New Lead or Deal Depending on CRM Mode](./how-to-add-objects-with-crm-mode.md) | [crm.settings.mode.get](../../../api-reference/crm/crm-settings-mode-get.md), [crm.deal.list](../../../api-reference/crm/deals/crm-deal-list.md),
[crm.activity.todo.add](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-add.md) | ID of the activity linked to the lead or deal ||
|| [Add Calendar Event for Client Interaction](./how-to-add-activity-to-contact.md) | [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md), [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) | ID of the activity of type "Event" ||
|| [How to Send an E-mail to a Client on Behalf of an Employee](./how-to-send-email.md) | [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md), [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) | ID of the activity of type "E-mail" ||
|| [How to Add a Template and Create a Document Based on It](./how-to-generate-documents.md) | [crm.documentgenerator.numerator.add](../../../api-reference/crm/document-generator/numerator/crm-document-generator-numerator-add.md), [crm.documentgenerator.template.add](../../../api-reference/crm/document-generator/templates/crm-document-generator-template-add.md), [crm.documentgenerator.document.add](../../../api-reference/crm/document-generator/documents/crm-document-generator-document-add.md) | ID of the document and link to the file ||
|#

## Configure Smart Processes

If the standard CRM objects are insufficient, use Smart Processes. These are custom CRM types with their own fields, funnels, and stages.

**Object Type (entityTypeId)**. A key parameter for working with Smart Processes. Obtain it before calling API methods, configuring fields, or comments.

**Funnels and Stages**. Set up the process for passing through stages. Create a funnel and add the necessary statuses.

**Custom Fields**. Expand the Smart Process card with your own fields. Configure the field type, number format, list options, and display parameters.

### Scenarios for Smart Processes

#|
|| **Scenario** | **Main Methods** | **Result** ||
|| [How to Add a Comment to the Smart Process Timeline](./how-to-add-comment-to-spa.md) | [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md), [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md), [crm.timeline.comment.add](../../../api-reference/crm/timeline/comments/crm-timeline-comment-add.md) | ID of the timeline record ||
|| [How to Create a Custom Field in a Smart Process](./how-to-add-user-field-to-spa.md) | [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md), [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md) | ID of the custom field ||
|| [How to Set Up Rounding for a Custom Field of Type "Number"](./how-to-add-precision-to-user-field.md) | [crm.userfield.settings.fields](../../../api-reference/crm/universal/user-defined-fields/crm-userfield-settings-fields.md), [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md), [userfieldconfig.update](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-update.md) | Updated settings for the numeric field ||
|| [How to Create a New Funnel with Stages in a Smart Process](./how-to-add-category-to-spa.md) | [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md), [crm.category.add](../../../api-reference/crm/universal/category/crm-category-add.md), [crm.status.add](../../../api-reference/crm/status/crm-status-add.md) | ID of the funnel and created stages ||
|#
