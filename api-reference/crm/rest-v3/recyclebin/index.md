# CRM Recycle Bin in REST 3.0: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note info "" %}

These methods belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

Deleted deals, leads, contacts, companies, and SPA items go to the CRM Recycle Bin if it is enabled for the object type. The methods of this group return Recycle Bin records and restore items from it.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Recycle Bin in CRM](https://helpdesk.bitrix24.com/open/24854300/)

## How to Get Started

1. Delete an item with the [crm.&lt;type&gt;.delete](../items/crm-entity-delete-rest-v3.md) method — it goes to the Recycle Bin
2. Find the Recycle Bin record with the [crm.recyclebin.list](./crm-recyclebin-list-rest-v3.md) method, for example, with the `[["entityTypeId","=",2],["title","=","Equipment supply"]]` filter. Note the `recordId` of the record
3. Restore the item with the [crm.recyclebin.restore](./crm-recyclebin-restore-rest-v3.md) method and this `recordId`
4. Take the new item ID from `result.item.id` in the response and work with the item using the [crm.&lt;type&gt;.*](../items/index.md) methods

## Specifics

- **Two IDs.** In a Recycle Bin record, `recordId` is the record ID, and `id` is the item ID before deletion. The [crm.recyclebin.restore](./crm-recyclebin-restore-rest-v3.md) method accepts `recordId`
- **New ID after restoration.** A restored item gets a new `id`. Update references to the item by its old ID in your data based on the method response
- **History is not carried over.** Records that the [change history](../history/index.md) methods store for the previous ID are not returned for the new ID
- **Which types go to the Recycle Bin.** Deals, leads, contacts, companies, and SPA items with the Recycle Bin enabled. Invoices and estimates are not covered by these methods. For an SPA, the Recycle Bin is enabled by the `isRecyclebinEnabled` type setting; its value is returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method
- **Retention period.** In cloud Bitrix24, records are available for 30 days after deletion; in the on-premise version, for the period set in the CRM Recycle Bin settings. The methods do not return or restore older records
- **Permissions.** An administrator sees and restores all CRM Recycle Bin records. Other users — only the items they deleted themselves
- **Plan.** If the Recycle Bin is not available on the Bitrix24 plan, [crm.recyclebin.list](./crm-recyclebin-list-rest-v3.md) returns an empty list, and [crm.recyclebin.restore](./crm-recyclebin-restore-rest-v3.md) returns the `RECYCLEBIN_ITEM_UNAVAILABLE` error
- **No permanent deletion.** There are no REST methods that delete a record from the Recycle Bin without restoring it. Records are deleted when the retention period expires or manually in the Recycle Bin interface

## Relationship with Other Objects

**CRM items.** The type of a deleted item is in the `entityTypeId` field of the Recycle Bin record: `1` — lead, `2` — deal, `3` — contact, `4` — company, or an SPA type ID. The list of types is returned by the [crm.dictionary.entitytype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method. A restored item is retrieved with the [crm.&lt;type&gt;.get](../items/crm-entity-get-rest-v3.md) method.

**Users.** The user who deleted the item is specified in the `deletedById` field. User data is returned by the [user.get](../../../user/user-get.md) method.

## Recycle Bin Record Fields

Record fields are described on the [crm.recyclebin.list](./crm-recyclebin-list-rest-v3.md#item) page. Field descriptions with the `filterable` and `sortable` attributes are returned by the [crm.recyclebin.field.list](./crm-recyclebin-field-list-rest-v3.md) and [crm.recyclebin.field.get](./crm-recyclebin-field-get-rest-v3.md) methods.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user; records depend on permissions

#|
|| **Method** | **Description** ||
|| [crm.recyclebin.list](./crm-recyclebin-list-rest-v3.md) | Returns CRM Recycle Bin records about deleted items ||
|| [crm.recyclebin.restore](./crm-recyclebin-restore-rest-v3.md) | Restores an item from the Recycle Bin by `recordId` ||
|| [crm.recyclebin.field.list](./crm-recyclebin-field-list-rest-v3.md) | Returns descriptions of Recycle Bin record fields ||
|| [crm.recyclebin.field.get](./crm-recyclebin-field-get-rest-v3.md) | Returns the description of a Recycle Bin record field by name ||
|#
