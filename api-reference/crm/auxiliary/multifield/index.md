# Multiple Fields: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Multiple fields are used for phone numbers, e-mails, and other contact information in leads, contacts, and companies.

> Quick Navigation: [All Methods](#all-methods)

## How to Populate a Multiple Field

1. Retrieve the composition of the fields and their characteristics using the method [crm.multifield.fields](./crm-multifield-fields.md)

2. Choose an allowed `VALUE_TYPE` value from the description of the [crm_multifield](../../data-types.md#crm_multifield) type

3. Pass the array of values to the field of the CRM object using a create or update method

4. Verify the saved data using the object reading method

## Linking Multiple Fields with CRM Objects

Contact details of a lead, a contact, and a company are stored in the multiple fields `PHONE`, `EMAIL`, `WEB`, and `IM`. The value of each field is an array of [crm_multifield](../../data-types.md#crm_multifield) objects.

**Lead.** Record and read contact details using the methods [crm.lead.add](../../leads/crm-lead-add.md), [crm.lead.update](../../leads/crm-lead-update.md), [crm.lead.get](../../leads/crm-lead-get.md).

**Contact.** Record and read contact details using the methods [crm.contact.add](../../contacts/crm-contact-add.md), [crm.contact.update](../../contacts/crm-contact-update.md), [crm.contact.get](../../contacts/crm-contact-get.md).

**Company.** Record and read contact details using the methods [crm.company.add](../../companies/crm-company-add.md), [crm.company.update](../../companies/crm-company-update.md), [crm.company.get](../../companies/crm-company-get.md).

## Example of Value Structure

```js
PHONE: [
    {
        VALUE: "555888",
        VALUE_TYPE: "MOBILE"
    }
],
EMAIL: [
    {
        VALUE: "client@example.com",
        VALUE_TYPE: "WORK"
    }
]
```

Most often, `PHONE` is passed a `VALUE_TYPE` of `MOBILE` or `WORK`, and `EMAIL` — `WORK` or `HOME`. For the complete set of allowed `VALUE_TYPE` values for a phone, an e-mail, a website, and a messenger, see the description of the [crm_multifield](../../data-types.md#crm_multifield) type.

{% note tip "Typical Use-Cases and Scenarios" %}

- [How to Change or Delete Phone Numbers and E-Mails](../../../../tutorials/crm/how-to-edit-crm-objects/how-to-change-email-or-phone.md)
- [Create a New Lead crm.lead.add](../../leads/crm-lead-add.md)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: any user

#|
|| **Method** | **Description** ||
|| [crm.multifield.fields](./crm-multifield-fields.md) | Returns the description of multiple fields ||
|#
