# Editing Data in CRM

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Editing CRM data involves changing values in cards and related objects: fields of leads, contacts, companies, and deals, as well as phone numbers and emails, task bindings, and payment dates.

A scenario is a sequence of requests for a single task. It describes the order of method calls and provides a code example.

> Quick navigation: [all scenarios](#choose-tutorial)

## Connection with CRM Objects

Scenarios are linked to customer cards, deals, tasks, custom fields, and payments.

- **Leads, contacts, companies, and deals.** Editing forms are built based on the object's fields. Basic operations are performed using the methods [crm.lead.*](../../../api-reference/crm/leads/index.md), [crm.contact.*](../../../api-reference/crm/contacts/index.md), [crm.company.*](../../../api-reference/crm/companies/index.md), and [crm.deal.*](../../../api-reference/crm/deals/index.md).
- **Phone numbers and emails.** Contact details are stored in fields of type [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield). To change or delete a value, pass a new array to the object update method.
- **Tasks.** Tasks are linked to CRM objects through bindings. You can retrieve a task using the method [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) and modify the binding using the methods [crm.activity.binding.*](../../../api-reference/crm/timeline/activities/binding/index.md).
- **Custom fields and payments.** The payment date can be retrieved using the method [crm.item.payment.list](../../../api-reference/crm/universal/payment/crm-item-payment-list.md) and saved in a custom field of the deal using the methods [crm.deal.userfield.*](../../../api-reference/crm/deals/user-defined-fields/index.md) and [crm.deal.update](../../../api-reference/crm/deals/crm-deal-update.md).

## How to Get Started

1. Identify the CRM object: lead, contact, company, or deal.
2. Choose a scenario from the table [How to choose a scenario](#choose-tutorial).
3. Check what access permissions and scopes are specified in the selected scenario.
4. Obtain the identifiers for the CRM object, deal, field, or payment needed for the scenario.
5. Execute the methods in the order described in the scenario.

## How to Choose a Scenario {#choose-tutorial}

#|
|| **If you need to** | **Open** ||
|| Create a form with lead fields and save changes | [How to create your lead editing card](./how-to-generate-edit-form-for-lead.md) ||
|| Create a form with contact fields and save changes | [How to create your contact editing card](./how-to-make-contact-edit-card.md) ||
|| Create a form with company fields and save changes | [How to create your company editing card](./how-to-generate-edit-form-for-company.md) ||
|| Create a form with deal fields and save changes | [How to create your deal editing card](./how-to-generate-edit-form-for-deal.md) ||
|| Update the list of phone numbers or emails in the contact card | [How to change or delete phone numbers and emails](./how-to-change-email-or-phone.md) ||
|| Move a task between two entities of the same type | [How to move a task between entities of the same type](./how-to-move-activity.md) ||
|| Move a task, for example, from a lead to a company | [How to move a task from one type of object to another](./how-to-move-activity-between-objects.md) ||
|| Record the payment date in a custom field of the deal | [How to save the payment date in the deal field](./how-to-set-paid-date-to-deal.md) ||
|#