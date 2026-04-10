# Multiple Fields: Overview of Methods

Multiple fields are used for phone numbers, email addresses, and other contact information in leads, contacts, and companies.

> Quick Navigation: [All Methods](#all-methods)

## Linking Multiple Fields with CRM Objects

**Lead, Contact, Company.** Contact details in these objects are stored in multiple fields. To correctly populate such fields in CRM methods, first retrieve their description via [crm.multifield.fields](./crm-multifield-fields.md).

**Value Types.** For each field, use the allowed values of `VALUE_TYPE` to accurately transmit contact information.

## How to Use Multiple Fields

1. Retrieve the structure and allowed value types using the [crm.multifield.fields](./crm-multifield-fields.md) method.
2. Prepare the field value in the [crm_multifield](../../data-types.md#crm_multifield) format.
3. Pass the prepared data to the method for creating or updating the desired CRM object.
4. Verify the saved data using the object reading method.

## Where to Use Multiple Fields

**Leads.** To record and read contact details, use the methods [crm.lead.add](../../leads/crm-lead-add.md), [crm.lead.update](../../leads/crm-lead-update.md), [crm.lead.get](../../leads/crm-lead-get.md).

**Contacts.** To record and read contact details, use the methods [crm.contact.add](../../contacts/crm-contact-add.md), [crm.contact.update](../../contacts/crm-contact-update.md), [crm.contact.get](../../contacts/crm-contact-get.md).

**Companies.** To record and read contact details, use the methods [crm.company.add](../../companies/crm-company-add.md), [crm.company.update](../../companies/crm-company-update.md), [crm.company.get](../../companies/crm-company-get.md).

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

For quick scenarios, `PHONE` is typically used with `VALUE_TYPE: MOBILE` or `WORK`, and for `EMAIL` — `WORK` or `HOME`. For a complete set of allowed values, see [crm.multifield.fields](./crm-multifield-fields.md).

{% note tip "Typical Use-Cases and Scenarios" %}

- [How to Change Phone Numbers and Email Using a Contact Example](../../../../tutorials/crm/how-to-edit-crm-objects/how-to-change-email-or-phone.md)
- [How to Create a New Lead](../../leads/crm-lead-add.md)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: any user

#| 
|| **Method** | **Description** ||
|| [crm.multifield.fields](./crm-multifield-fields.md) | Returns the description of multiple fields ||
|#