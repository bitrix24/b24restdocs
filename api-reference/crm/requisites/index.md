# Details in CRM: Overview of Methods

Details are separate CRM entities that store data used in closing deals: Tax Identification Number (TIN), Tax Registration Reason Code (TRRC), Primary State Registration Number (PSRN), banking details, and addresses. Everything necessary for document templates.

The `Details` field is available:
* in contacts and companies — where buyer data is stored
* in your companies — where your company's data, acting as the seller, is stored. This type of company is located in a separate section in the CRM settings

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [add company details](https://helpdesk.bitrix24.com/open/5870771/), [use of company details](https://helpdesk.bitrix24.com/open/16059544/) 

## Connection of Details with Other CRM Entities

**Contact.** The default details template is Person.

**Company.** The default details template is Company.

**My Company.** The details of the company selected as the main seller are automatically filled in all documents.

**Address.** A separate entity accessible through the standard `Address` field. Addresses can only be linked to Details or Leads. To work with addresses, use the group of methods [crm.address.*](./addresses/index.md), where the details ID is passed in the `ENTITY_ID` parameter.  

**Documents.** Any printed forms: invoices, acts, contracts, and others generated through the document generator. 

{% note tip "User Documentation" %}

- [Connections of requisites with CRM entities](../requisites/links/index.md)
- [Updates in working with addresses and contact or company details in CRM](https://helpdesk.bitrix24.com/open/11785262/)
- [Documents in CRM: how to create and send in a couple of minutes](https://helpdesk.bitrix24.com/open/19441484/)

{% endnote %}

## Details Templates

Any detail for a contact or company is created within templates. A template consists of a set of fields characteristic of a specific type of legal and natural persons:
* the Company template consists of fields for legal entities such as LLC: `VAT ID`, `Company Name`
* the Person template consists of other fields: `First Name`, `Last Name`

You can add, modify, and delete details templates through the group of methods [crm.requisite.preset.*](./presets/index.md).

Manage the list of fields for a specific template — [crm.requisite.preset.field.*](./presets/fields/index.md). 

Create and modify custom fields that can be used in the template — [crm.requisite.userfield.*](./user-fields/index.md).

{% note tip "User Documentation" %}

- [Contact or company details templates](https://helpdesk.bitrix24.com/open/5869609/)

{% endnote %}

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute methods: depending on the method

### Basic

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [crm.requisite.add](./universal/crm-requisite-add.md) | Creates a new requisite ||
    || [crm.requisite.update](./universal/crm-requisite-update.md) | Updates an existing requisite ||
    || [crm.requisite.get](./universal/crm-requisite-get.md) | Returns a requisite by ID ||
    || [crm.requisite.list](./universal/crm-requisite-list.md) | Returns a list of requisites by filter ||
    || [crm.requisite.delete](./universal/crm-requisite-delete.md) | Deletes a requisite and all associated objects ||
    || [crm.requisite.fields](./universal/crm-requisite-fields.md) | Returns the description of requisite fields ||
    |#

- Events

    #| 
    || **Event** | **Triggered** ||
    || [onCrmRequisiteAdd](./events/on-crm-requisite-add.md) | When a requisite is added ||
    || [onCrmRequisiteUpdate](./events/on-crm-requisite-update.md) | When a requisite is modified ||
    || [onCrmRequisiteDelete](./events/on-crm-requisite-delete.md) | When a requisite is deleted ||
    |#

{% endlist %}

### Addresses

{% list tabs %}

- Methods 

    #| 
    || **Method** | **Description** ||
    || [crm.address.add](./addresses/crm-address-add.md) | Adds a new address for a requisite or lead ||
    || [crm.address.update](./addresses/crm-address-update.md) | Modifies an address for a requisite or lead ||
    || [crm.address.list](./addresses/crm-address-list.md) | Returns a list of addresses by filter ||
    || [crm.address.delete](./addresses/crm-address-delete.md) | Deletes an address ||
    || [crm.address.fields](./addresses/crm-address-fields.md) | Returns the formal description of address fields ||
    |#

- Events
  
   #| 
    || **Event** | **Triggered** ||
    || [onCrmAddressRegister](./events/on-crm-address-register.md) | When an address is registered ||
    || [onCrmAddressUnregister](./events/on-crm-address-unregister.md) | When an address is deleted ||
    |#

{% endlist %}

### Banking Details

{% list tabs %}

- Methods 

    #| 
    || **Method** | **Description** ||
    || [crm.requisite.bankdetail.add](./bank-detail/crm-requisite-bank-detail-add.md) | Creates a new banking requisite ||
    || [crm.requisite.bankdetail.update](./bank-detail/crm-requisite-bank-detail-update.md) | Modifies an existing banking requisite ||
    || [crm.requisite.bankdetail.get](./bank-detail/crm-requisite-bank-detail-get.md) | Returns a banking requisite by ID ||
    || [crm.requisite.bankdetail.list](./bank-detail/crm-requisite-bank-detail-list.md) | Returns a list of banking requisites by filter ||
    || [crm.requisite.bankdetail.delete](./bank-detail/crm-requisite-bank-detail-delete.md) | Deletes a banking requisite ||
    || [crm.requisite.bankdetail.fields](./bank-detail/crm-requisite-bank-detail-fields.md) | Returns the formal description of banking requisite fields ||
    |#

- Events
  
   #| 
    || **Event** | **Triggered** ||
    || [onCrmBankDetailAdd](./events/on-crm-bank-detail-add.md) | When a banking requisite is added ||
    || [onCrmBankDetailUpdate](./events/on-crm-bank-detail-update.md) | When a banking requisite is modified ||
    || [onCrmBankDetailDelete](./events/on-crm-bank-detail-delete.md) | When a banking requisite is deleted ||
    |#

{% endlist %}

### Custom Requisite Fields

{% list tabs %}

- Methods 

    #| 
    || **Method** | **Description** ||
    || [crm.requisite.userfield.add.md](./user-fields/crm-requisite-userfield-add.md) | Creates a new custom field for a requisite ||
    || [crm.requisite.userfield.update.md](./user-fields/crm-requisite-userfield-update.md) | Modifies an existing custom field for a requisite ||
    || [crm.requisite.userfield.get.md](./user-fields/crm-requisite-userfield-get.md) | Returns a custom field for a requisite by ID ||
    || [crm.requisite.userfield.list.md](./user-fields/crm-requisite-userfield-list.md) | Returns a list of custom fields for a requisite by filter ||
    || [crm.requisite.userfield.delete.md](./user-fields/crm-requisite-userfield-delete.md) | Deletes a custom field for a requisite ||
    |#

- Events
  
   #| 
    || **Event** | **Triggered** ||
    || [onCrmRequisiteUserFieldAdd](./events/on-crm-requisite-user-field-add.md) | When a custom field is added ||
    || [onCrmRequisiteUserFieldUpdate](./events/on-crm-requisite-user-field-update.md) | When a custom field is modified ||
    || [onCrmRequisiteUserFieldDelete](./events/on-crm-requisite-user-field-delete.md) | When a custom field is deleted ||
    || [onCrmRequisiteUserFieldSetEnumValues](./events/on-crm-requisite-user-field-set-enum-values.md) | When the set of values for a custom field of list type is modified ||
    |#

{% endlist %}

### Connections of Requisites with CRM Entities

#| 
|| **Method** | **Description** ||
|| [crm.requisite.link.register](./links/crm-requisite-link-register.md) | Registers the connection of requisites with an entity ||
|| [crm.requisite.link.get](./links/crm-requisite-link-get.md) | Returns the connection of requisites with an entity ||
|| [crm.requisite.link.list](./links/crm-requisite-link-list.md) | Returns a list of requisites connections by filter ||
|| [crm.requisite.link.unregister](./links/crm-requisite-link-unregister.md) | Deletes the connection of requisites with an entity ||
|| [crm.requisite.link.fields](./links/crm-requisite-link-fields.md) | Returns the formal description of fields for requisites connections ||
|#

### Requisites Templates

#| 
|| **Method** | **Description** ||
|| [crm.requisite.preset.add](./presets/crm-requisite-preset-add.md) | Creates a new requisites template ||
|| [crm.requisite.preset.update](./presets/crm-requisite-preset-update.md) | Modifies a requisites template ||
|| [crm.requisite.preset.countries](./presets/crm-requisite-preset-countries.md) | Returns a possible list of countries for requisites templates ||
|| [crm.requisite.preset.get](./presets/crm-requisite-preset-get.md) | Returns a requisites template by ID ||
|| [crm.requisite.preset.list](./presets/crm-requisite-preset-list.md) | Returns a list of requisites templates by filter ||
|| [crm.requisite.preset.delete](./presets/crm-requisite-preset-delete.md) | Deletes a requisites template ||
|| [crm.requisite.preset.fields](./presets/crm-requisite-preset-fields.md) | Returns the formal description of fields for requisites templates ||
|#

### Fields of Requisites Templates

#| 
|| **Method** | **Description** ||
|| [crm.requisite.preset.field.add](./presets/fields/crm-requisite-preset-field-add.md) | Adds a customizable field to the requisites template ||
|| [crm.requisite.preset.field.update](./presets/fields/crm-requisite-preset-field-update.md) | Modifies a customizable field in the requisites template ||
|| [crm.requisite.preset.field.availabletoadd](./presets/fields/crm-requisite-preset-field-available-to-add.md) | Returns fields available for addition to the specified requisites template ||
|| [crm.requisite.preset.field.get](./presets/fields/crm-requisite-preset-field-get.md) | Returns the description of a customizable field in the requisites template by ID ||
|| [crm.requisite.preset.field.list](./presets/fields/crm-requisite-preset-field-list.md) | Returns a list of all customizable fields for a specific requisites template ||
|| [crm.requisite.preset.field.delete](./presets/fields/crm-requisite-preset-field-delete.md) | Deletes a customizable field from the requisites template ||
|| [crm.requisite.preset.field.fields](./presets/fields/crm-requisite-preset-field-fields.md) | Returns the formal description of fields that describe the customizable field in the requisites template ||
|#