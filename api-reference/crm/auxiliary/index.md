# Helper Objects: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Helper methods tell you which values to pass to the main CRM methods. For example, to find a client's legal addresses, you need the address type ID `6`, which the [crm.enum.addresstype](./enum/crm-enum-address-type.md) method returns. To write a phone number, you need to know which fields it consists of — the [crm.multifield.fields](./multifield/crm-multifield-fields.md) method describes them. These methods do not change data in CRM.

Helper methods are split into two groups: [multiple fields](./multifield/index.md) and [enumerations](./enum/index.md).

> Quick navigation: [all methods](#all-methods)

## Multiple Fields

Multiple fields store the contact details of leads, contacts, and companies: phone numbers, e-mails, messengers. The value of such a field is an array of [crm_multifield](../data-types.md#crm_multifield) objects. For example, this is how the [crm.contact.get](../contacts/crm-contact-get.md) method returns the `PHONE` field:

```json
{
    "PHONE": [
        {
            "ID": "6137",
            "VALUE_TYPE": "WORK",
            "VALUE": "+493012345678",
            "TYPE_ID": "PHONE"
        }
    ]
}
```

The [crm.multifield.fields](./multifield/crm-multifield-fields.md) method returns a description of these four fields: the data type, the name, and the read-only flag.

Which methods accept the value and which value types are allowed — see the [Multiple Fields](./multifield/index.md) subsection.

{% note tip "Typical use-cases and scenarios" %}

- [How to change or delete phone numbers and e-mails](../../../tutorials/crm/how-to-edit-crm-objects/how-to-change-email-or-phone.md)
- [How to add a lead via a web form](../../../tutorials/crm/how-to-add-crm-objects/how-to-add-lead.md)

{% endnote %}

## Enumerations

Enumerations are reference lists of identifiers that CRM uses in the parameters of other methods. Most [enumeration](./enum/index.md) methods return an array of items with the identifier `ID`, the name `NAME`, and the symbolic codes `SYMBOL_CODE` and `SYMBOL_CODE_SHORT`. For example, the method [crm.enum.ownertype](./enum/crm-enum-owner-type.md) returns the identifiers of CRM object types and SPAs for the `entityTypeId` parameter, while the method [crm.enum.addresstype](./enum/crm-enum-address-type.md) returns the identifiers of address types: legal, physical, and shipping addresses.

Which identifier belongs in which parameter — see the [Enumerations](./enum/index.md) subsection.

{% note tip "Typical use-cases and scenarios" %}

- [How to add a comment to the smart process timeline](../../../tutorials/crm/how-to-add-crm-objects/how-to-add-comment-to-spa.md)
- [How to get a client's address from CRM](../../../tutorials/crm/how-to-get-lists/how-to-get-address.md)

{% endnote %}

## Where to Get VAT Rates

VAT rates are managed by the group of methods [catalog.vat.*](../../catalog/vat/index.md) of the Product Catalog. This group has its own scope `catalog`, so it is not included in the method tables below.

The [catalog.vat.list](../../catalog/vat/catalog-vat-list.md) method returns the identifier `id` and the percentage `rate` for each rate. Pass:

- the percentage `rate` in the `taxRate` parameter of the group of methods [crm.item.productrow.*](../universal/product-rows/index.md) — to set the VAT for a product in a deal or another CRM object. The rate identifier does not work here: CRM treats it as a percentage
- the rate identifier `id` in the `vatId` parameter of the group of methods [catalog.product.*](../../catalog/product/index.md) — to set the VAT for a product or service in the product catalog

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the methods: a user with read access to leads, deals, or other CRM objects, including those in digital workspaces. The [crm.enum.getorderownertypes](./enum/crm-enum-get-order-owner-types.md) method can be called by any user

### Multiple Fields

#|
|| **Method** | **Description** ||
|| [crm.multifield.fields](./multifield/crm-multifield-fields.md) | Returns the description of multiple fields ||
|#

### Enumerations

#|
|| **Method** | **Description** ||
|| [crm.enum.fields](./enum/crm-enum-fields.md) | Returns the description of the fields of enumeration items ||
|| [crm.enum.getorderownertypes](./enum/crm-enum-get-order-owner-types.md) | Returns the identifiers of object types to which order binding is available ||
|| [crm.enum.ownertype](./enum/crm-enum-owner-type.md) | Returns the types of objects in CRM ||
|| [crm.enum.addresstype](./enum/crm-enum-address-type.md) | Returns the types of addresses ||
|| [crm.enum.settings.mode](./enum/crm-enum-settings-mode.md) | Returns the description of CRM operation modes ||
|#

### Deprecated Enumerations

The methods below are no longer being developed. The activity enumeration values are listed in the [CRM data types](../data-types.md#activity-enums) reference, and working with activities is covered in the [Activities in CRM](../timeline/activities/index.md) section.

#|
|| **Method** | **Description** ||
|| [crm.enum.activitytype](./enum/outdated/crm-enum-activity-type.md) | Returns the enumeration items "Activity Types" ||
|| [crm.enum.activitypriority](./enum/outdated/crm-enum-activity-priority.md) | Returns the enumeration items "Activity Priorities" ||
|| [crm.enum.activitydirection](./enum/outdated/crm-enum-activity-direction.md) | Returns the enumeration items "Activity Direction" for e-mails and calls ||
|| [crm.enum.activitynotifytype](./enum/outdated/crm-enum-activity-notify-type.md) | Returns the enumeration items "Activity Start Notification Type" for meetings and calls ||
|| [crm.enum.activitystatus](./enum/outdated/crm-enum-activity-status.md) | Returns the enumeration items "Status" ||
|| [crm.enum.contenttype](./enum/outdated/crm-enum-content-type.md) | Returns the enumeration items "Description Type" ||
|#
