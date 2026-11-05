# Migrating to the New CRM Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

This page helps you migrate an integration from the CRM methods of the previous API version to REST 3.0: find the equivalent of each method, take into account what REST 3.0 does not support yet, and update the request and response format.

{% note info "" %}

Methods of both versions work at the same time and change the same data. You can migrate an integration step by step: call some methods at the `/rest/api/` address and the rest at the `/rest/` address.

{% endnote %}

## Migration Steps {#migration-steps}

1. Find the equivalents of the methods your integration uses by following the rules in the [How to Find the Equivalent of a Method](#methods) section. If REST 3.0 does not support a task, keep that call in the previous version: such tasks are listed in the [What REST 3.0 Does Not Support Yet](#previous-only) section
2. Change the call address from `/rest/` to `/rest/api/`. The method name stays the same for deals, leads, contacts, companies, and estimates. For SPAs and invoices, it changes to `crm.<type>.<action>`, for example `crm.smartprocess1038.add`
3. Rename the fields to `camelCase`: `TITLE` → `title`, `STAGE_ID` → `stageId`. The field names of each type are listed on the fields pages, for example [Deal fields](./rest-v3/items/fields-deal.md)
4. List the fields you need in the `select` parameter. Without it, the `get` and `list` methods return only `id`
5. Update the handling of responses and errors: data comes in the `result` object, and errors come in the `error` object with a code and a message. The format is described in the [REST 3.0 overview](../rest-v3.md)
6. Test the new calls on test data before switching over the production scenario

## Example: Creating a Deal {#example}

In the previous version, fields are passed in uppercase, and the method returns the ID as a number.

```bash
curl -X POST \
-H "Content-Type: application/json" \
-d '{"FIELDS":{"TITLE":"Purchase request","ASSIGNED_BY_ID":1,"OPPORTUNITY":150000,"CURRENCY_ID":"EUR"}}' \
https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.add
```

```json
{
    "result": 9021
}
```

In REST 3.0, the same call goes to the `/rest/api/` address, fields are passed in `camelCase`, and the ID comes in the `result` object.

```bash
curl -X POST \
-H "Content-Type: application/json" \
-d '{"fields":{"title":"Purchase request","assignedById":1,"opportunity":150000,"currencyId":"EUR"}}' \
https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.add
```

```json
{
    "result": {
        "id": 9023
    }
}
```

## How to Find the Equivalent of a Method {#methods}

**Methods of deals, leads, contacts, companies, and estimates.** The method name in REST 3.0 is the same, and only the call address changes. For example, `crm.deal.add`, `crm.deal.get`, `crm.deal.list`, `crm.deal.update`, and `crm.deal.delete` exist in both versions, as do the `add`, `get`, `list`, `update`, and `delete` methods of leads, contacts, companies, and estimates.

**Universal methods `crm.item.*`.** In REST 3.0, the object type is part of the method name, so the `entityTypeId` parameter is not needed. For example, `crm.item.add` with `entityTypeId: 2` becomes the `crm.deal.add` method, and `crm.item.list` with `entityTypeId: 1038` becomes the `crm.smartprocess1038.list` method. Product items of an item also get the type in the name: for a deal, `crm.item.productrow.add` becomes `crm.deal.productrow.add`. The segments of all types are listed in the [CRM overview](./index.md#method-name).

**Other methods have different names in REST 3.0.** They are listed in the table. The table uses deal methods as examples, and company methods for requisites and addresses. For other object types, the name contains their segment instead of `deal` or `company`: `crm.lead.field.list`, `crm.contact.requisite.add`.

#|
|| **Previous version** | **REST 3.0** ||
|| `crm.deal.fields` | [crm.deal.field.list](./rest-v3/fields/crm-entity-field-list-rest-v3.md) ||
|| `crm.deal.userfield.add`, `get`, `list`, `update`, `delete` | [crm.deal.field.custom.add](./rest-v3/fields/crm-entity-field-custom-add-rest-v3.md), [get](./rest-v3/fields/crm-entity-field-custom-get-rest-v3.md), [list](./rest-v3/fields/crm-entity-field-custom-list-rest-v3.md), [update](./rest-v3/fields/crm-entity-field-custom-update-rest-v3.md), [delete](./rest-v3/fields/crm-entity-field-custom-delete-rest-v3.md) ||
|| `crm.deal.productrows.get` | [crm.deal.productrow.list](./rest-v3/product-rows/crm-entity-product-row-list-rest-v3.md) ||
|| `crm.deal.productrows.set` | [crm.deal.productrow.replace](./rest-v3/product-rows/crm-entity-product-row-replace-rest-v3.md) ||
|| `crm.deal.details.configuration.get` | [crm.deal.detailpagelayout.get](./rest-v3/detail-page-layout/crm-entity-detail-page-layout-get-rest-v3.md) ||
|| `crm.deal.details.configuration.set` | [crm.deal.detailpagelayout.update](./rest-v3/detail-page-layout/crm-entity-detail-page-layout-update-rest-v3.md) ||
|| `crm.deal.details.configuration.forceCommonScopeForAll` | [crm.deal.detailpagelayout.setcommonforall](./rest-v3/detail-page-layout/crm-entity-detail-page-layout-set-common-for-all-rest-v3.md) ||
|| `crm.deal.contact.items.get` | [crm.deal.get](./rest-v3/items/crm-entity-get-rest-v3.md): all contacts in the `contactsId` field, the primary contact in the `contactId` field ||
|| `crm.deal.contact.add` | [crm.deal.addfieldvalue](./rest-v3/items/crm-entity-add-field-value-rest-v3.md) with the `contactsId` field. The primary contact is set by [crm.deal.setprimarycontact](./rest-v3/items/crm-entity-set-primary-contact-rest-v3.md) ||
|| `crm.deal.contact.delete` | [crm.deal.deletefieldvalue](./rest-v3/items/crm-entity-delete-field-value-rest-v3.md) with the `contactsId` field ||
|| `crm.deal.contact.items.set` | [crm.deal.update](./rest-v3/items/crm-entity-update-rest-v3.md) with the `contactsId` array. The first contact becomes the primary one ||
|| `crm.deal.contact.items.delete` | [crm.deal.update](./rest-v3/items/crm-entity-update-rest-v3.md) with an empty `contactsId` array ||
|| `crm.contact.company.items.get` | [crm.contact.get](./rest-v3/items/crm-entity-get-rest-v3.md): all companies in the `companiesId` field, the primary company in the `companyId` field ||
|| `crm.contact.company.add` | [crm.contact.addfieldvalue](./rest-v3/items/crm-entity-add-field-value-rest-v3.md) with the `companiesId` field. The company is added as non-primary ||
|| `crm.contact.company.delete` | [crm.contact.deletefieldvalue](./rest-v3/items/crm-entity-delete-field-value-rest-v3.md) with the `companiesId` field ||
|| `crm.contact.company.items.set` | [crm.contact.update](./rest-v3/items/crm-entity-update-rest-v3.md) with the `companiesId` array. The first company becomes the primary one ||
|| `crm.contact.company.items.delete` | [crm.contact.update](./rest-v3/items/crm-entity-update-rest-v3.md) with an empty `companiesId` array ||
|| `crm.company.contact.items.get` | [crm.company.get](./rest-v3/items/crm-entity-get-rest-v3.md): all contacts in the `contactsId` field. The primary contact flag of the company is not returned ||
|| `crm.company.contact.add` | [crm.company.addfieldvalue](./rest-v3/items/crm-entity-add-field-value-rest-v3.md) with the `contactsId` field ||
|| `crm.company.contact.delete` | [crm.company.deletefieldvalue](./rest-v3/items/crm-entity-delete-field-value-rest-v3.md) with the `contactsId` field ||
|| `crm.company.contact.items.set` | [crm.company.update](./rest-v3/items/crm-entity-update-rest-v3.md) with the `contactsId` array ||
|| `crm.company.contact.items.delete` | [crm.company.update](./rest-v3/items/crm-entity-update-rest-v3.md) with an empty `contactsId` array ||
|| `crm.category.*` | [crm.deal.category.*](./rest-v3/categories/index.md) ||
|| `crm.status.*` | Directories — [crm.dictionary.&lt;directory&gt;.*](./rest-v3/dictionaries/index.md), stages of deals and SPAs — [crm.deal.category.stage.*](./rest-v3/categories/index.md) ||
|| `crm.requisite.*` | [crm.company.requisite.*](./rest-v3/requisites/index.md) and `crm.contact.requisite.*` ||
|| `crm.requisite.bankdetail.*` | [crm.company.requisite.bankrequisite.*](./rest-v3/requisites/index.md) ||
|| `crm.requisite.link.*` | [crm.deal.clientrequisite.*](./rest-v3/requisites/index.md) for client requisites, `crm.deal.mycompanyrequisite.*` for my company requisites ||
|| `crm.address.*` | Addresses of contacts, companies, and leads — [crm.company.address.*](./rest-v3/addresses/index.md), requisite addresses — [crm.company.requisite.address.*](./rest-v3/requisites/index.md) ||
|| `crm.activity.binding.add`, `delete` | [crm.activity.binding.bind](./rest-v3/activities/crm-activity-binding-bind-rest-v3.md), [crm.activity.binding.unbind](./rest-v3/activities/crm-activity-binding-unbind-rest-v3.md) ||
|| `crm.enum.addresstype` | [crm.dictionary.addresstype.list](./rest-v3/dictionaries/crm-dictionary-list-rest-v3.md) ||
|#

## What REST 3.0 Does Not Support Yet {#previous-only}

For these tasks, keep the calls of the previous version at the `/rest/` address.

#|
|| **Task** | **Method of the previous version** ||
|| Work with CRM documents and B2E documents | [crm.item.*](./universal/index.md) ||
|| Work with pipelines of invoices and documents | [crm.category.*](./universal/category/index.md) ||
|| Work with stages of leads, estimates, and invoices, and with the call list status directory | [crm.status.*](./status/index.md) ||
|| Create a custom field of the List or File type | `crm.deal.userfield.add`, `crm.lead.userfield.add`, `crm.contact.userfield.add`, `crm.company.userfield.add`, `crm.quote.userfield.add` ||
|| Set the order of contacts in a deal, or make a contact's company primary when adding it | `crm.deal.contact.add`, `crm.lead.contact.add`, `crm.quote.contact.add`, `crm.contact.company.add`, `crm.company.contact.add` ||
|| Work with custom fields of requisites | [crm.requisite.userfield.*](./requisites/user-fields/index.md) ||
|| Create and update requisite templates | [crm.requisite.preset.*](./requisites/presets/index.md) ||
|| Select my company requisites in an estimate or an SPA | [crm.requisite.link.*](./requisites/links/index.md) ||
|| Retrieve all activity fields, including settings and provider data, or retrieve a list of activities without administrator permissions | [crm.activity.get](./timeline/activities/activity-base/crm-activity-get.md), [crm.activity.list](./timeline/activities/activity-base/crm-activity-list.md) ||
|| Create or update an activity | [Timeline and Activities](./timeline/index.md) ||
|| Reset a card view to the default settings | `crm.deal.details.configuration.reset`, `crm.lead.details.configuration.reset`, `crm.contact.details.configuration.reset`, `crm.company.details.configuration.reset`, `crm.item.details.configuration.reset` ||
|| Retrieve the short object type codes `D`, `L`, `C` | [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md) ||
|| Find out the CRM mode without administrator permissions | [crm.settings.mode.get](./crm-settings-mode-get.md) ||
|| Create or configure an SPA type | [crm.type.*](./universal/user-defined-object-types/index.md) ||
|| Work with the timeline, document generator, automation, currencies, call lists, duplicates, sales intelligence, digital workplaces, payments, and deliveries | The methods of these sections in the [CRM overview](./index.md#all-methods) ||
|#

## How REST 3.0 Differs from the Previous Version {#differences}

- **One scheme for all types.** The `crm.<type>.*` methods work the same way for deals, leads, contacts, companies, estimates, invoices, and SPAs. Only the set of groups and the fields of the type differ
- **Field names in `camelCase`.** The deal stage is the `stageId` field, not `STAGE_ID`. Custom fields keep names like `UF_CRM_…`
- **Fields are selected explicitly.** Without `select`, the `crm.<type>.get` and `crm.<type>.list` methods return only the item `id`. In `select`, you can request fields of linked objects with dot notation, for example `assignedBy.lastName` or `stage.name`. Nested groups have their own `select` rules, which are described on the method pages
- **New filter and pagination format.** The `filter`, `order`, and `pagination` parameters follow the general REST 3.0 rules. The `crm.<type>.list` method returns 50 items by default, up to 1000. In list methods of nested groups, the filter usually requires a condition on the owner, for example `ownerId`, and the page limit may be lower
- **Field descriptions in every group.** Each group of methods has its own `field.get` and `field.list`, for example `crm.deal.productrow.field.list` returns the fields of a product item

The parameter and response format is described in the [REST 3.0 overview](../rest-v3.md): [selecting fields and linked objects](../rest-v3.md#connection), [filtering](../rest-v3.md#filter), [comparison with the old version](../rest-v3.md#table).
