# Multiple Fields: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

In multiple fields, CRM stores the phone numbers, e-mail addresses, websites, and messenger contacts of leads, contacts, and companies. A single field can hold several values — for example, a contact's work and mobile phone numbers. Each value has its own subtype: `WORK`, `MOBILE`, and others.

> Quick Navigation: [All Methods](#all-methods)

## How to Populate a Multiple Field

1. Retrieve the description of the `ID`, `TYPE_ID`, `VALUE`, and `VALUE_TYPE` fields using the method [crm.multifield.fields](./crm-multifield-fields.md): the data type, the name, and the read-only flag

2. Choose an allowed `VALUE_TYPE` value from the [Table of Values](./crm-multifield-fields.md#value-type): it depends on `TYPE_ID`

3. Pass the values to the field of the CRM object using a create or update method

4. To change a retained number or address, pass the new value together with the ID of the existing one, returned by the method that retrieves the object. In the deprecated methods, the ID is specified in the `ID` field inside the element, and in the [crm.item.update](../../universal/crm-item-update.md) method, it is used as the element key in the `fm` object, as in the [Example](../../../../tutorials/crm/how-to-edit-crm-objects/how-to-change-email-or-phone.md#fm-format)

5. Verify the retained data by retrieving the object

{% note warning "" %}

Without the ID, the method does not replace the existing value but retains the new one alongside it. It does not add duplicates: if the `PHONE` field already contains this number with the `WORK` subtype, a second identical entry does not appear.

{% endnote %}

Write permissions are checked by the method you use to create or update the object. For the number of characters you can pass in a value, see [CRM Field Length Limits](../../field-length-limits.md#related-data), and for how often you can call methods, see [REST API Limits](../../../../settings/performance/limits.md).

## Linking Multiple Fields with CRM Objects

How you pass contact details depends on the methods you use: universal or deprecated.

**Universal methods.** The [crm.item.add](../../universal/crm-item-add.md), [crm.item.update](../../universal/crm-item-update.md), and [crm.item.get](../../universal/crm-item-get.md) methods work with contact details in the `fm` field. When creating an object, pass an array of objects with the `typeId`, `valueType`, and `value` keys to this field, for example `{"typeId": "PHONE", "valueType": "WORK", "value": "+493012345678"}`. In the read response, each value also has an `id`.

**Deprecated methods.** The development of the lead, contact, and company methods has been halted. They accept and return contact details in the `PHONE`, `EMAIL`, `WEB`, and `IM` fields. The value of each field is an array of [crm_multifield](../../data-types.md#crm_multifield) objects. Methods for each object:

- lead — [crm.lead.add](../../leads/crm-lead-add.md), [crm.lead.update](../../leads/crm-lead-update.md), [crm.lead.get](../../leads/crm-lead-get.md)
- contact — [crm.contact.add](../../contacts/crm-contact-add.md), [crm.contact.update](../../contacts/crm-contact-update.md), [crm.contact.get](../../contacts/crm-contact-get.md)
- company — [crm.company.add](../../companies/crm-company-add.md), [crm.company.update](../../companies/crm-company-update.md), [crm.company.get](../../companies/crm-company-get.md)

## Example of Value Structure

This is how a phone number and an e-mail are passed in the deprecated methods:

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

For `PHONE`, the most common `VALUE_TYPE` values are `MOBILE` and `WORK`; for `EMAIL`, they are `WORK` and `HOME`. For the complete list of `VALUE_TYPE` values for a phone, an e-mail, a website, and a messenger, see the description of the [crm.multifield.fields](./crm-multifield-fields.md#value-type) method.

{% note tip "Typical Use-Cases and Scenarios" %}

- [How to Change or Delete Phone Numbers and E-Mails](../../../../tutorials/crm/how-to-edit-crm-objects/how-to-change-email-or-phone.md)
- [How to Add a Lead via a Web Form](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-lead.md)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: a user with read access to leads, deals, or other CRM objects, including those in digital workspaces

#|
|| **Method** | **Description** ||
|| [crm.multifield.fields](./crm-multifield-fields.md) | Returns the description of multiple fields ||
|#
