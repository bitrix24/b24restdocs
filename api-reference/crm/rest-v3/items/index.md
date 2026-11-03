# CRM Items in REST 3.0: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note info "" %}

These methods belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The methods in this group create, update, retrieve, and delete CRM items: deals, leads, contacts, companies, estimates, invoices, my companies, and SPA items. The object type is part of the method name: `crm.deal.add` creates a deal, `crm.lead.add` creates a lead, and `crm.smartprocess1038.add` creates an SPA item with the type ID `1038`.

> Quick navigation: [all methods](#all-methods)

## Object Types and Method Names

The documentation has one page per action that covers all object types. Each page starts with a table that lists the method name for every type.

#|
|| **Object type** | **Segment in the method name** | **Example** | **Fields** ||
|| Deal | `deal` | `crm.deal.add` | [Deal fields](./fields-deal.md) ||
|| Lead | `lead` | `crm.lead.add` | [Lead fields](./fields-lead.md) ||
|| Contact | `contact` | `crm.contact.add` | [Contact fields](./fields-contact.md) ||
|| Company | `company` | `crm.company.add` | [Company fields](./fields-company.md) ||
|| Estimate | `quote` | `crm.quote.add` | [Estimate fields](./fields-quote.md) ||
|| Invoice | `smartinvoice` | `crm.smartinvoice.add` | [Invoice fields](./fields-smart-invoice.md) ||
|| My company | `mycompany` | `crm.mycompany.add` | [My company fields](./fields-my-company.md) ||
|| SPA | `smartprocess<entityTypeId>` | `crm.smartprocess1038.add` | [SPA fields](./fields-smart-process.md) ||
|#

The SPA type ID `entityTypeId` is returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method. The method name is written in lowercase: `crm.smartinvoice.add`, not `crm.smartInvoice.add`.

The set of methods available for an SPA depends on the settings of its type. For example, if client linking is disabled for the type, it has no `setprimarycontact` method. The fields that depend on the settings are described on the [SPA fields](./fields-smart-process.md) page.

## Getting Started

1. Look up the fields of the object type on its fields page or with the [crm.&lt;type&gt;.field.list](../fields/crm-entity-field-list-rest-v3.md) method. The method shows which fields can be edited, filtered, and sorted
2. Create an item with the [crm.&lt;type&gt;.add](./crm-entity-add-rest-v3.md) method. Leads, companies, and my companies require the `title` field, and contacts require `name` or `lastName`
3. Retrieve an item with the [crm.&lt;type&gt;.get](./crm-entity-get-rest-v3.md) method or a list of items with the [crm.&lt;type&gt;.list](./crm-entity-list-rest-v3.md) method. Without the `select` parameter, these methods return only `id`: list the fields you need in `select`
4. Update an item with the [crm.&lt;type&gt;.update](./crm-entity-update-rest-v3.md) method. To add or remove a single value of a multiple field, such as a phone number or a contact, use [crm.&lt;type&gt;.addfieldvalue](./crm-entity-add-field-value-rest-v3.md) and [crm.&lt;type&gt;.deletefieldvalue](./crm-entity-delete-field-value-rest-v3.md)
5. Delete an item with the [crm.&lt;type&gt;.delete](./crm-entity-delete-rest-v3.md) method

## My Company

My company is the company on whose behalf you work with clients: its requisites are filled in on invoices and documents. The `crm.company.*` methods do not return my companies, and the `crm.mycompany.*` methods do not return client companies.

The `crm.mycompany.*` methods are available only to a CRM administrator. To read the name, contacts, and requisites of my company without administrator permissions, use [crm.mycompany.basic.get](./crm-my-company-basic-get-rest-v3.md) and [crm.mycompany.basic.list](./crm-my-company-basic-list-rest-v3.md): they require only the permission to view companies, invoices, or documents.

## Relationship with Other Objects

A CRM item is linked to pipelines, custom fields, contacts and companies, products, requisites, activities, and the card view.

**Pipelines and stages.** The item's pipeline is stored in the `categoryId` field, and its stage in `stageId`. Retrieve the values with the [crm.&lt;type&gt;.category.list](../categories/crm-entity-category-list-rest-v3.md) and [crm.&lt;type&gt;.category.stage.list](../categories/crm-entity-category-stage-list-rest-v3.md) methods. Deals, contacts, companies, and SPAs have pipelines; deals and SPAs have stages.

**Custom fields.** Values are passed in the same `fields` object by field name, for example, `UF_CRM_1687529546`. The fields themselves are created and updated by the [crm.&lt;type&gt;.field.custom.*](../fields/crm-entity-field-custom-list-rest-v3.md) group of methods.

**Contacts and companies.** Deals, leads, estimates, invoices, and SPA items are linked to contacts via the `contactsId` field and to a company via `companyId`. The primary contact is set by the [crm.&lt;type&gt;.setprimarycontact](./crm-entity-set-primary-contact-rest-v3.md) method.

**Product rows.** Products of deals, leads, estimates, invoices, and SPAs are stored separately from the item fields. They are handled by the [crm.&lt;type&gt;.productrow.*](../product-rows/crm-entity-product-row-list-rest-v3.md) group of methods.

**Requisites and addresses.** Requisites and addresses of contacts and companies are separate objects. Their methods are described in the [crm.&lt;type&gt;.requisite.*](../requisites/crm-entity-requisite-list-rest-v3.md) and [crm.&lt;type&gt;.address.*](../addresses/crm-entity-address-list-rest-v3.md) groups.

**Activities.** The item's activities are returned by the [crm.&lt;type&gt;.activity.list](../activities/crm-entity-activity-list-rest-v3.md) method.

**Item card.** The fields and blocks a user sees in the item card are defined by [card views](../detail-page-layout/index.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permissions for items of this CRM object type. For my company, a CRM administrator

### Main

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.add](./crm-entity-add-rest-v3.md) | Creates an item ||
|| [crm.&lt;type&gt;.update](./crm-entity-update-rest-v3.md) | Updates an item ||
|| [crm.&lt;type&gt;.get](./crm-entity-get-rest-v3.md) | Returns an item by ID ||
|| [crm.&lt;type&gt;.list](./crm-entity-list-rest-v3.md) | Returns a list of items with filtering, sorting, and pagination ||
|| [crm.&lt;type&gt;.delete](./crm-entity-delete-rest-v3.md) | Deletes an item ||
|| [crm.&lt;type&gt;.aggregate](./crm-entity-aggregate-rest-v3.md) | Counts items by filter ||
|#

### Multiple Fields and Contacts

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.addfieldvalue](./crm-entity-add-field-value-rest-v3.md) | Adds a value to a multiple field and keeps the other values ||
|| [crm.&lt;type&gt;.deletefieldvalue](./crm-entity-delete-field-value-rest-v3.md) | Removes a value from a multiple field and keeps the other values ||
|| [crm.&lt;type&gt;.setprimarycontact](./crm-entity-set-primary-contact-rest-v3.md) | Makes a linked contact primary ||
|#

### My Company

#|
|| **Method** | **Description** ||
|| [crm.mycompany.basic.get](./crm-my-company-basic-get-rest-v3.md) | Returns the name, contacts, and requisites of my company ||
|| [crm.mycompany.basic.list](./crm-my-company-basic-list-rest-v3.md) | Returns a list of my companies with contacts and requisites ||
|#

### Object Fields

#|
|| **Page** | **Description** ||
|| [Deal fields](./fields-deal.md) | Deal fields, their types and attributes ||
|| [Lead fields](./fields-lead.md) | Lead fields, their types and attributes ||
|| [Contact fields](./fields-contact.md) | Contact fields, their types and attributes ||
|| [Company fields](./fields-company.md) | Company fields, their types and attributes ||
|| [Estimate fields](./fields-quote.md) | Estimate fields, their types and attributes ||
|| [Invoice fields](./fields-smart-invoice.md) | Invoice fields, their types and attributes ||
|| [My company fields](./fields-my-company.md) | My company fields, their types and attributes ||
|| [SPA fields](./fields-smart-process.md) | SPA fields and how they depend on the type settings ||
|#
