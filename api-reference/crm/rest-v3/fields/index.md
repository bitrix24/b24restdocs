# CRM Fields in REST 3.0: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note info "" %}

These methods belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

Fields store the data of a CRM card, for example, the name and amount of a deal. The methods of this group show which fields items have and help you add your own. For example, you can create a "Contract number" field in deals and make it required at the approval stage. You can fill in this field when you create a deal with the [crm.deal.add](../items/crm-entity-add-rest-v3.md) method or later with the [crm.deal.update](../items/crm-entity-update-rest-v3.md) method.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Custom Fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)

## Object Types

The object type is part of the method name: [crm.deal.field.custom.add](./crm-entity-field-custom-add-rest-v3.md) creates a deal field, and [crm.smartprocess1038.field.custom.add](./crm-entity-field-custom-add-rest-v3.md) creates a field of the SPA with the type ID `1038`.

#|
|| **Object type** | **Segment in the method name** | **Fields** ||
|| Deal | `deal` | [Deal fields](../items/fields-deal.md) ||
|| Lead | `lead` | [Lead fields](../items/fields-lead.md) ||
|| Contact | `contact` | [Contact fields](../items/fields-contact.md) ||
|| Company | `company` | [Company fields](../items/fields-company.md) ||
|| Estimate | `quote` | [Estimate fields](../items/fields-quote.md) ||
|| Invoice | `smartinvoice` | [Invoice fields](../items/fields-smart-invoice.md) ||
|| My company | `mycompany` | [My company fields](../items/fields-my-company.md) ||
|| SPA | `smartprocess<entityTypeId>` | [SPA fields](../items/fields-smart-process.md) ||
|#

My company has only the field description methods: [crm.mycompany.field.get](./crm-entity-field-get-rest-v3.md) and [crm.mycompany.field.list](./crm-entity-field-list-rest-v3.md). It has no methods for custom fields, list field options, or field requirements.

The type ID `entityTypeId` of your SPA is returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method. SPAs have field methods regardless of the type settings, for example, when pipelines and stages are disabled in the type.

The method name is written in lowercase: `crm.smartinvoice.field.list`, not `crm.smartInvoice.field.list`.

## How to Get Started

1. Retrieve the fields of the object type with the [crm.&lt;type&gt;.field.list](./crm-entity-field-list-rest-v3.md) method. The method shows which fields can be edited, filtered, and sorted
2. Create a custom field with the [crm.&lt;type&gt;.field.custom.add](./crm-entity-field-custom-add-rest-v3.md) method. The method returns the field description with its `id` and the name `name`
3. Pass the value of the new field in `fields` of the [crm.&lt;type&gt;.add](../items/crm-entity-add-rest-v3.md) and [crm.&lt;type&gt;.update](../items/crm-entity-update-rest-v3.md) methods, with the field name as the key. To retrieve the value, specify the field name in `select` of the [crm.&lt;type&gt;.get](../items/crm-entity-get-rest-v3.md) method
4. If the field must be filled in at certain stages, configure this with the [crm.&lt;type&gt;.field.required.set](./crm-entity-field-required-set-rest-v3.md) method
5. Update the field settings with the [crm.&lt;type&gt;.field.custom.update](./crm-entity-field-custom-update-rest-v3.md) method or delete the field with the [crm.&lt;type&gt;.field.custom.delete](./crm-entity-field-custom-delete-rest-v3.md) method

## List Fields

A list field stores a set of options, for example, the priorities "High", "Medium", and "Low". The [crm.&lt;type&gt;.field.custom.add](./crm-entity-field-custom-add-rest-v3.md) method does not create a list field of the `enumeration` type. You can create one, for example, in the Bitrix24 interface or with the [userfieldconfig.add](../../universal/userfieldconfig/userfieldconfig-add.md) method, and add, update, and delete its options with the [crm.&lt;type&gt;.field.custom.enum.*](./crm-entity-field-custom-enum-list-rest-v3.md) methods.

The [crm.&lt;type&gt;.field.custom.enum.list](./crm-entity-field-custom-enum-list-rest-v3.md) method returns the options of a single field: it requires a filter by `fieldId`.

## Field Requirements

A field can be made required in two ways:

- with the `isMandatory` attribute of a custom field in the [crm.&lt;type&gt;.field.custom.add](./crm-entity-field-custom-add-rest-v3.md) and [crm.&lt;type&gt;.field.custom.update](./crm-entity-field-custom-update-rest-v3.md) methods — the field is required in all pipelines
- with the [crm.&lt;type&gt;.field.required.set](./crm-entity-field-required-set-rest-v3.md) method — the field is required in the selected pipeline as a whole or at its selected stages. For a lead and an estimate, the requirement applies to the entire object type. This method can also configure standard fields, for example, `opportunity`

Requirements are checked by the item card in Bitrix24: without a required field, a user cannot save the item or move it to a stage. By default, REST 3.0 item methods do not check requirements: [crm.&lt;type&gt;.add](../items/crm-entity-add-rest-v3.md) creates an item without such a field, and [crm.&lt;type&gt;.update](../items/crm-entity-update-rest-v3.md) moves it to a stage where the field is required. To make the methods check them, enable the "Validate required custom fields in REST" option in the CRM settings.

## Permissions

Field descriptions and list field options can be retrieved by a user with permission to read items of this object type: the [crm.&lt;type&gt;.field.get](./crm-entity-field-get-rest-v3.md), [crm.&lt;type&gt;.field.list](./crm-entity-field-list-rest-v3.md), [crm.&lt;type&gt;.field.custom.enum.get](./crm-entity-field-custom-enum-get-rest-v3.md), and [crm.&lt;type&gt;.field.custom.enum.list](./crm-entity-field-custom-enum-list-rest-v3.md) methods.

The other methods of the group require the "Edit CRM preferences" permission in CRM. This also applies to reading custom fields with the [crm.&lt;type&gt;.field.custom.get](./crm-entity-field-custom-get-rest-v3.md) and [crm.&lt;type&gt;.field.custom.list](./crm-entity-field-custom-list-rest-v3.md) methods.

## Relationship with Other Objects

Fields are linked to items, pipelines, and stages.

**Items.** Field values are stored in CRM items. The methods of the [crm.&lt;type&gt;.*](../items/index.md) group pass and return them by field name: a standard field in camelCase, for example, `title`, and a custom field, for example, `UF_CRM_CONTRACT_NUMBER`.

**Pipelines and stages.** The [crm.&lt;type&gt;.field.required.set](./crm-entity-field-required-set-rest-v3.md) method sets the requirement separately for each pipeline and its stages. Pipeline and stage IDs of deals and SPAs are returned by the [crm.&lt;type&gt;.category.list](../categories/crm-entity-category-list-rest-v3.md) and [crm.&lt;type&gt;.category.stage.list](../categories/crm-entity-category-stage-list-rest-v3.md) methods. Where to get the pipelines and stages of invoices, leads, and estimates is described on the [crm.&lt;type&gt;.field.required.set](./crm-entity-field-required-set-rest-v3.md#category-id) page.

## Methods of the Previous Version

At the `/rest/` address, fields are handled by the methods of several groups:

- custom fields of a lead, deal, contact, company, and estimate — the `crm.<type>.userfield.*` methods, for example, [crm.deal.userfield.*](../../deals/user-defined-fields/index.md)
- custom fields of any object — the [userfieldconfig.*](../../universal/userfieldconfig/index.md) methods, where the object is set by the `moduleId` and `entityId` parameters
- item field descriptions — the [crm.item.fields](../../universal/crm-item-fields.md) method, where the object type is set by the `entityTypeId` parameter

In REST 3.0, these tasks are handled by the methods of a single `crm.<type>.field.*` group, and the object type is part of the method name.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### Field Descriptions

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.field.get](./crm-entity-field-get-rest-v3.md) | Returns a field description by name ||
|| [crm.&lt;type&gt;.field.list](./crm-entity-field-list-rest-v3.md) | Returns the object type fields and their attributes: whether a field can be edited, filtered, and sorted ||
|#

### Custom Fields

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.field.custom.add](./crm-entity-field-custom-add-rest-v3.md) | Creates a custom field ||
|| [crm.&lt;type&gt;.field.custom.update](./crm-entity-field-custom-update-rest-v3.md) | Updates the settings of a custom field ||
|| [crm.&lt;type&gt;.field.custom.get](./crm-entity-field-custom-get-rest-v3.md) | Returns a custom field by ID or name ||
|| [crm.&lt;type&gt;.field.custom.list](./crm-entity-field-custom-list-rest-v3.md) | Returns a list of custom fields ||
|| [crm.&lt;type&gt;.field.custom.delete](./crm-entity-field-custom-delete-rest-v3.md) | Deletes a custom field together with its values ||
|#

### List Field Options

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.field.custom.enum.add](./crm-entity-field-custom-enum-add-rest-v3.md) | Adds an option to a list field ||
|| [crm.&lt;type&gt;.field.custom.enum.update](./crm-entity-field-custom-enum-update-rest-v3.md) | Updates a list field option ||
|| [crm.&lt;type&gt;.field.custom.enum.get](./crm-entity-field-custom-enum-get-rest-v3.md) | Returns an option by ID ||
|| [crm.&lt;type&gt;.field.custom.enum.list](./crm-entity-field-custom-enum-list-rest-v3.md) | Returns the options of a list field ||
|| [crm.&lt;type&gt;.field.custom.enum.delete](./crm-entity-field-custom-enum-delete-rest-v3.md) | Deletes an option from a list field ||
|#

### Field Requirements

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.field.required.get](./crm-entity-field-required-get-rest-v3.md) | Returns the field requirement in a pipeline ||
|| [crm.&lt;type&gt;.field.required.set](./crm-entity-field-required-set-rest-v3.md) | Makes a field required in a pipeline or at stages ||
|| [crm.&lt;type&gt;.field.required.clear](./crm-entity-field-required-clear-rest-v3.md) | Removes the field requirement in a pipeline ||
|#
