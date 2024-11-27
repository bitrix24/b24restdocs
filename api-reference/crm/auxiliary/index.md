# Overview of Auxiliary Methods

Auxiliary method groups include multiple fields, enumerations, and VAT rates.

> Quick navigation: [all methods and events](#all-methods)

## Multiple Fields

The method [crm.multifield.fields](./multifield/crm-multifield-fields.md) returns information about the structure of multiple fields, such as phone numbers or e-mails. To populate a field with a value [of a type](../data-types.md#crm_multifield), pass the data in the structure returned by the method. 
Example of passing data to fill in a mobile phone number:

```js
PHONE: [
            { 
                VALUE: "555888",
                VALUE_TYPE: "MOBILE",
            },
        ] ,
```

{% note tip "Typical use-cases and scenarios" %}

- [How to change phone numbers and e-mails using a contact example](../../../tutorials/crm/how-to-edit-crm-objects/how-to-change-email-or-phone.md)
- [Create a new lead crm.lead.add](../leads/crm-lead-add.md)

{% endnote %}

## Enumerations

The group of enumeration methods [crm.enum.*](./enum/index.md) returns information about the names and identifiers of CRM objects. For example, the method [crm.enum.ownertype](./enum/crm-enum-owner-type.md) returns identifiers for smart processes, while the method [crm.enum.addresstype](./enum/crm-enum-address-type.md) returns identifiers for address types: legal, physical, delivery address.

{% note tip "Typical use-cases and scenarios" %}

- [How to add a comment to the timeline of a smart process](../../../tutorials/crm/how-to-add-crm-objects/how-to-add-comment-to-spa.md)
- [How to get a client's address from CRM](../../../tutorials/crm/how-to-get-lists/how-to-get-address.md)

{% endnote %}

## VAT Rates

The group of methods [crm.vat.*](./vat/index.md) manages VAT rates. These methods allow you to [create](./vat/crm-vat-add.md), [delete](./vat/crm-vat-delete.md), [update](./vat/crm-vat-update.md), and [retrieve](./vat/crm-vat-list.md) VAT rate values.

To set the VAT for a product in a deal or another CRM object, use the `taxRate` parameter from the group of methods [crm.item.productrow.*](../universal/product-rows/index.md).

To set the VAT for a product or service in the product catalog, use the `vatId` parameter from the group of methods [catalog.product.*](../../catalog/product/index.md).

{% note tip "Typical use-cases and scenarios" %}

- [Add a deal (lead, invoice, estimate) with products, applying discounts and taxes](../../../tutorials/crm/how-to-add-crm-objects/how-to-product-binding.md)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute methods: depending on the method

### Multiple Fields

#|
|| **Method** | **Description** ||
|| [crm.multifield.fields](./multifield/crm-multifield-fields.md) | Returns the description of multiple fields ||
|#

### Enumerations

#|
|| **Method** | **Description** ||
|| [crm.enum.fields](./enum/crm-enum-fields.md) | Returns the description of enumeration fields ||
|| [crm.enum.ownertype](./enum/crm-enum-owner-type.md) | Returns the enumeration items for "Owner Type" ||
|| [crm.enum.getorderownertypes](./enum/crm-enum-get-order-owner-types.md) | Returns the identifiers of entity types to which order binding is available ||
|| [crm.enum.contenttype](./enum/crm-enum-content-type.md) | Returns the enumeration items for "Content Type" ||
|| [crm.enum.activitytype](./enum/crm-enum-activity-type.md) | Returns the enumeration items for "Activity Type" ||
|| [crm.enum.activitypriority](./enum/crm-enum-activity-priority.md) | Returns the enumeration items for "Activity Priority" ||
|| [crm.enum.activitydirection](./enum/crm-enum-activity-direction.md) | Returns the enumeration items for "Activity Direction" (for e-mails and calls) ||
|| [crm.enum.activitynotifytype](./enum/crm-enum-activity-notify-type.md) | Returns the enumeration items for "Activity Start Notification Type" (for meetings and calls) ||
|| [crm.enum.addresstype](./enum/crm-enum-address-type.md) | Returns the enumeration items for "Address Type" ||
|| [crm.enum.activitystatus](./enum/crm-enum-activity-status.md) | Returns the enumeration items for "Status" (STATUS) ||
|| [crm.enum.settings.mode](./enum/crm-enum-settings-mode.md) | Returns the description of CRM operation modes ||
|#

### VAT Rates

#|
|| **Method** | **Description** ||
|| [crm.vat.add](./vat/crm-vat-add.md) | Creates a new VAT rate ||
|| [crm.vat.delete](./vat/crm-vat-delete.md) | Deletes a VAT rate ||
|| [crm.vat.get](./vat/crm-vat-get.md) | Returns the VAT rate by identifier ||
|| [crm.vat.fields](./vat/crm-vat-fields.md) | Returns the description of VAT rate fields ||
|| [crm.vat.list](./vat/crm-vat-list.md) | Returns a list of VAT rates ||
|| [crm.vat.update](./vat/crm-vat-update.md) | Updates an existing VAT rate ||
|#