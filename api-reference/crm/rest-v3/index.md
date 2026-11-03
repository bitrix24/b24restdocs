# CRM in REST 3.0: Overview of Sections and Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note info "" %}

These methods belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

CRM methods in REST 3.0 work with CRM items and everything around them: fields, pipelines and stages, product items, requisites, addresses, activities, card views, and directories. For example, you can create a deal, add products to it, link the client's requisites, and retrieve the deal's activities — all through the `/rest/api/` address.

The method names are the same as in the old version: `crm.deal.add` exists both in the old REST and in REST 3.0. The version is determined by the call address. A call to `/rest/` runs the old version of the method, and a call to `/rest/api/` runs the REST 3.0 method. Both versions work at the same time.

> Quick navigation: [all sections and methods](#all-methods)
>
> User documentation: [CRM implementation steps](https://helpdesk.bitrix24.com/open/23477678/)

## Object Type in the Method Name {#method-name}

The object type is part of the method name: `crm.<type>.<action>`. For example, `crm.deal.add` creates a deal, and `crm.smartprocess1038.add` creates an SPA item with the type ID `1038`. That is why the documentation has one page per action for all types, and each page starts with a table that lists the full method name for every type.

#|
|| **Object type** | **Segment in the method name** | **Example** ||
|| Deal | `deal` | `crm.deal.list` ||
|| Lead | `lead` | `crm.lead.list` ||
|| Contact | `contact` | `crm.contact.list` ||
|| Company | `company` | `crm.company.list` ||
|| Estimate | `quote` | `crm.quote.list` ||
|| Invoice | `smartinvoice` | `crm.smartinvoice.list` ||
|| My company | `mycompany` | `crm.mycompany.list` ||
|| CRM document | `smartdocument` | `crm.smartdocument.productrow.list` ||
|| B2E document | `smartb2edocument` | `crm.smartb2edocument.detailpagelayout.list` ||
|| SPA | `smartprocess<entityTypeId>` | `crm.smartprocess1038.list` ||
|#

The SPA type ID `entityTypeId` is returned by the [crm.type.list](../universal/user-defined-object-types/crm-type-list.md) method. REST 3.0 has no equivalent of this method, so call it at the old version address `/rest/`.

The method name is written in lowercase: `crm.smartinvoice.add` and `crm.deal.productrow.list`, not `crm.smartInvoice.add` and `crm.deal.productRow.list`. A method with different casing or with a type that does not exist, for example `crm.smartprocess99999.add`, returns the `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION` error with status 404.

## Methods Available for Each Object Type

Not every group of methods is available for every type. For example, only deals and SPAs have stages, and only contacts and companies have requisites.

#|
|| **Method group** | **deal** | **lead** | **contact** | **company** | **quote** | **smartinvoice** | **mycompany** | **smartdocument** | **smartb2edocument** | **smartprocess** ||
|| [Items](./items/index.md) | + | + | + | + | + | + | + | — | — | + ||
|| Primary contact `setprimarycontact` | + | + | — | — | + | + | — | + | + | + ||
|| [Field descriptions](./fields/index.md) `field.get`, `field.list` | + | + | + | + | + | + | + | — | — | + ||
|| [Custom fields and required fields](./fields/index.md) | + | + | + | + | + | + | — | — | — | + ||
|| [Pipelines](./categories/index.md) | + | — | + | + | — | — | — | — | — | + ||
|| [Stages](./categories/index.md) | + | — | — | — | — | — | — | — | — | + ||
|| [Product items](./product-rows/index.md) | + | + | — | — | + | + | — | + | — | + ||
|| [Requisites and bank details](./requisites/index.md) | — | — | + | + | — | — | — | — | — | — ||
|| [Client requisites](./requisites/index.md) | + | — | — | — | + | + | — | + | — | + ||
|| [My company requisites](./requisites/index.md) | + | — | — | — | — | + | — | — | — | — ||
|| [Addresses](./addresses/index.md) | — | + | + | + | — | — | — | — | — | — ||
|| [Card views](./detail-page-layout/index.md) | + | + | + | + | + | + | — | + | + | + ||
|| [Activities](./activities/index.md) | + | + | + | + | + | + | — | + | — | + ||
|#

[Directories](./dictionaries/index.md) and [lead mode](./lead-mode/index.md) are not tied to an object type: `crm.dictionary.<directory>.*`, `crm.lead.isenabled`, `crm.lead.enable`, `crm.lead.disable`.

The exact list of methods available in your Bitrix24 is returned by the [OpenAPI documentation](../../rest-v3.md#openapi).

### SPA Methods Depend on the Type Settings

For an SPA, a "+" in the table means that the group is available when the corresponding type setting is enabled:

- without pipelines `isCategoriesEnabled`, there are no pipeline or stage methods
- without stages `isStagesEnabled`, there are no stage methods
- without products `isLinkWithProductsEnabled`, there are no product item methods
- without client linking `isClientEnabled`, there is no `setprimarycontact` method

The set of fields also depends on the settings. The fields that disappear without each setting are described on the [SPA fields](./items/fields-smart-process.md) page.

## Getting Started

1. Determine the object type and the method name from the [Object Type in the Method Name](#method-name) table. For an SPA, retrieve `entityTypeId` with the [crm.type.list](../universal/user-defined-object-types/crm-type-list.md) method
2. Look up the fields of the type on its fields page, for example [Deal fields](./items/fields-deal.md), or with the [crm.&lt;type&gt;.field.list](./fields/crm-entity-field-list-rest-v3.md) method
3. Create an item with the [crm.&lt;type&gt;.add](./items/crm-entity-add-rest-v3.md) method
4. Retrieve items with the [crm.&lt;type&gt;.list](./items/crm-entity-list-rest-v3.md) method. Without the `select` parameter, the `get` and `list` methods return only `id`, so list the fields you need in `select`
5. Add products, requisites, or addresses to the item with the methods of the corresponding group from the [overview of methods](#all-methods)

## How REST 3.0 Differs from the Old CRM Methods

- **One scheme for all types.** The `crm.<type>.*` methods work the same way for deals, leads, contacts, companies, estimates, invoices, and SPAs. Only the set of groups and the fields of the type differ
- **Field names in `camelCase`.** The deal stage is the `stageId` field, not `STAGE_ID`. Custom fields keep names like `UF_CRM_…`
- **Fields are selected explicitly.** Without `select`, the `crm.<type>.get` and `crm.<type>.list` methods return only the item `id`. In `select`, you can request fields of linked objects with dot notation, for example `assignedBy.lastName` or `stage.name`. Nested groups have their own `select` rules, which are described on the method pages
- **New filter and pagination format.** The `filter`, `order`, and `pagination` parameters follow the general REST 3.0 rules. The `crm.<type>.list` method returns 50 items by default, up to 1000. In list methods of nested groups, the filter usually requires a condition on the owner, for example `ownerId`, and the page limit may be lower
- **Field descriptions in every group.** Each group of methods has its own `field.get` and `field.list`, for example `crm.deal.productrow.field.list` returns the fields of a product item

The parameter and response format is described in the [REST 3.0 overview](../../rest-v3.md): [selecting fields and linked objects](../../rest-v3.md#connection), [filtering](../../rest-v3.md#filter), [comparison with the old version](../../rest-v3.md#table).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#|
|| **Section** | **What the methods do** ||
|| [Items](./items/index.md) | Create, update, retrieve, count, and delete CRM items, work with multiple fields and the primary contact. This section also has the fields page for each object type ||
|| [Field Management](./fields/index.md) | Return field descriptions, create custom fields and list values, and set required fields ||
|| [Pipelines and Stages](./categories/index.md) | Create and configure pipelines and stages of deals and SPAs, and pipelines of contacts and companies ||
|| [Product Items](./product-rows/index.md) | Add, update, and replace products in deals, leads, estimates, invoices, documents, and SPAs ||
|| [Requisites](./requisites/index.md) | Work with requisites, bank details, and requisite addresses of contacts and companies, and link client and my company requisites to items ||
|| [Addresses](./addresses/index.md) | Work with addresses of contacts, companies, and leads ||
|| [Card Views](./detail-page-layout/index.md) | Configure the sections and fields of the item card, select the view for a user, and set the common view ||
|| [Activities](./activities/index.md) | Retrieve, complete, and delete activities, manage activity links to items, and work with emails ||
|| [Directories](./dictionaries/index.md) | Return and modify CRM directories: deal and contact types, sources, salutations, industries, and others ||
|| [Lead Mode](./lead-mode/index.md) | Check, enable, and disable leads in CRM ||
|| [CRM Forms](../form/index.md) | Create, configure, enable, and delete CRM forms, and retrieve the code for embedding a form on a website ||
|#
