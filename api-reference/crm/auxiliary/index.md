# Helper Objects: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Helper methods return reference data required to work with the main CRM objects: the structure of multiple fields and enumeration values — identifiers of object types, address types, and activities. These methods do not modify any data of their own.

Helper methods are split into two groups: [multiple fields](./multifield/index.md) and [enumerations](./enum/index.md).

> Quick navigation: [all methods](#all-methods)

## Multiple Fields

Multiple fields store the contact details of leads, contacts, and companies: phone numbers, e-mails, messengers. The value of such a field is an array of [crm_multifield](../data-types.md#crm_multifield) objects. The method [crm.multifield.fields](./multifield/crm-multifield-fields.md) returns the composition and characteristics of this object's fields.

Which methods accept the value and which value types are allowed — see the [Multiple Fields](./multifield/index.md) subsection.

{% note tip "Typical use-cases and scenarios" %}

- [How to change or delete phone numbers and e-mails](../../../tutorials/crm/how-to-edit-crm-objects/how-to-change-email-or-phone.md)
- [Create a new lead crm.lead.add](../leads/crm-lead-add.md)

{% endnote %}

## Enumerations

Enumerations are reference lists of identifiers that CRM uses in the parameters of other methods. The group of methods [crm.enum.*](./enum/index.md) returns identifier–name pairs. For example, the method [crm.enum.ownertype](./enum/crm-enum-owner-type.md) returns the identifiers of CRM object types and smart processes for the `entityTypeId` parameter, while the method [crm.enum.addresstype](./enum/crm-enum-address-type.md) returns the identifiers of address types: legal, actual, delivery address.

Which identifier belongs in which parameter — see the [Enumerations](./enum/index.md) subsection.

{% note tip "Typical use-cases and scenarios" %}

- [How to add a comment to the smart process timeline](../../../tutorials/crm/how-to-add-crm-objects/how-to-add-comment-to-spa.md)
- [How to get a client's address from CRM](../../../tutorials/crm/how-to-get-lists/how-to-get-address.md)

{% endnote %}

## Where to Get VAT Rates

VAT rates are managed by the group of methods [catalog.vat.*](../../catalog/vat/index.md) of the Product Catalog. This group has its own scope `catalog`, so it is not included in the method tables below.

Pass the rate identifier returned by the `catalog.vat.*` methods:

- in the `taxRate` parameter of the group of methods [crm.item.productrow.*](../universal/product-rows/index.md) — to set the VAT for a product in a deal or another CRM object
- in the `vatId` parameter of the group of methods [catalog.product.*](../../catalog/product/index.md) — to set the VAT for a product or service in the product catalog

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can perform methods: any user

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

The methods below are no longer being developed. Retrieve activity enumerations in the [Activities in CRM](../timeline/activities/index.md) section.

#|
|| **Method** | **Description** ||
|| [crm.enum.activitytype](./enum/outdated/crm-enum-activity-type.md) | Returns the enumeration items "Activity Types" ||
|| [crm.enum.activitypriority](./enum/outdated/crm-enum-activity-priority.md) | Returns the enumeration items "Activity Priorities" ||
|| [crm.enum.activitydirection](./enum/outdated/crm-enum-activity-direction.md) | Returns the enumeration items "Activity Direction" for e-mails and calls ||
|| [crm.enum.activitynotifytype](./enum/outdated/crm-enum-activity-notify-type.md) | Returns the enumeration items "Activity Start Notification Type" for meetings and calls ||
|| [crm.enum.activitystatus](./enum/outdated/crm-enum-activity-status.md) | Returns the enumeration items "Status" ||
|| [crm.enum.contenttype](./enum/outdated/crm-enum-content-type.md) | Returns the enumeration items "Description Type" ||
|#
