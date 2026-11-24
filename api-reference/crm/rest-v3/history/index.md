# Change History in REST 3.0: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note info "" %}

These methods belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The methods of this group read the history of CRM items: who changed the item fields, when, and how, and how the item moved through stages and pipelines. The records are read-only: there are no methods to create, update, or delete history.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [CRM History](https://helpdesk.bitrix24.com/open/25803275/)

## How to Choose a Method

#|
|| **Task** | **Method** ||
|| Find out which item fields changed, who changed them and when, and what the values were before and after | [crm.&lt;type&gt;.history.list](./crm-entity-history-list-rest-v3.md) ||
|| Retrieve the path of an item through stages with stage and pipeline codes, for example, to calculate the time spent at each stage | [crm.&lt;type&gt;.stagehistory.list](./crm-entity-stage-history-list-rest-v3.md) ||
|#

The change history also contains stage changes, but its values are stage names, as on the "History" tab of the card. For stage calculations, use the stage history: it contains the `stageId` and `categoryId` codes.

## Object Types and Method Names

The object type is set by the `<type>` segment of the method name.

#|
|| **Object type** | **Segment in the method name** | **Change history** | **Stage history** ||
|| Deal | `deal` | Yes | Yes ||
|| Lead | `lead` | Yes | Yes ||
|| Contact | `contact` | Yes | No ||
|| Company | `company` | Yes | No ||
|| Invoice | `smartinvoice` | Yes | Yes ||
|| SPA | `smartprocess<entityTypeId>`, for example, `smartprocess1038` | Yes | Only if stages are enabled in the type ||
|#

Estimates, my company, and documents have no history methods. SPA type IDs `entityTypeId` are returned by the [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) method. Method names are written in lowercase: `crm.smartinvoice.stagehistory.list`.

## How to Get Started

1. Retrieve the item ID, for example, with the [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md) method
2. Retrieve the change history of the item with the [crm.&lt;type&gt;.history.list](./crm-entity-history-list-rest-v3.md) method and the `[["ownerId","=",<ID>]]` filter. To retrieve changes of a single field, add the `["fieldName","=","STAGE_ID"]` condition
3. Retrieve the stage transitions of the item with the [crm.&lt;type&gt;.stagehistory.list](./crm-entity-stage-history-list-rest-v3.md) method and the same filter
4. To collect changes of all items of a type for a period, pass both period boundaries instead of `ownerId`: `[["createdTime",">=","2026-11-01T00:00:00+01:00"],["createdTime","<","2026-12-01T00:00:00+01:00"]]`. A page contains up to 1000 records; request the next pages with `pagination`

## Specifics

- **The filter is required.** Pass a condition on `ownerId` or both `createdTime` period boundaries: the start with the `>=` operator and the end with `<`. The filter accepts no other fields, operators, or condition groups with `"logic": "or"`
- **Only field change records.** `history.list` does not return records about creating, deleting, and linking an item or events added manually
- **Values are text.** `oldValue` and `newValue` in `history.list` are the text from the "History" tab: stage names, `Yes` and `No`, dates in the format of the Bitrix24 language, amounts with HTML entities
- **No change source.** On the "History" tab, a record shows who made the change: an automation rule, a REST app, an import, or AI. REST 3.0 methods do not return this attribute
- **Deleted items.** The history of an item in the Recycle Bin is not available. An item restored with the [crm.recyclebin.restore](../recyclebin/crm-recyclebin-restore-rest-v3.md) method gets a new ID, and the earlier records are not returned for it
- **Permissions.** The user needs permission to view the item and permission to view history in a CRM access role. For SPAs of a digital workplace — permission to view history of this digital workplace. Records of items the user cannot view are not included in the response

## Relationship with Other Objects

**CRM items.** A history record belongs to the item from the `ownerId` field, and the item type is in `ownerTypeId`. The item is retrieved with the [crm.&lt;type&gt;.get](../items/crm-entity-get-rest-v3.md) method.

**Stages and pipelines.** Stage and pipeline codes in the stage history are decoded by the [crm.&lt;type&gt;.category.stage.list](../categories/crm-entity-category-stage-list-rest-v3.md) and [crm.&lt;type&gt;.category.list](../categories/crm-entity-category-list-rest-v3.md) methods for deals and SPAs. Lead and invoice stages are returned by the previous-version method [crm.status.list](../../status/crm-status-list.md).

**Users.** The author of a change is in the `authorId` field of the change history. User data is returned by the [user.get](../../../user/user-get.md) method.

**Recycle Bin.** Deleted items and their restoration are described in the [CRM Recycle Bin](../recyclebin/index.md) section.

The stage history in the previous API version is the [crm.stagehistory.list](../../crm-stage-history-list.md) method.

## Field Methods

Descriptions of history record fields are returned by the `crm.<type>.history.field.list` and `crm.<type>.history.field.get` methods, and of stage history record fields — by `crm.<type>.stagehistory.field.list` and `crm.<type>.stagehistory.field.get`. They work like other REST 3.0 field methods: `field.list` accepts `select` with a list of description attributes, and `field.get` also accepts `name` with the field name, for example, `{"name":"fieldName"}`. The `filterable` and `sortable` attributes in the response show which fields can be passed in `filter` and `order`. The fields themselves are described in the tables on the [crm.&lt;type&gt;.history.list](./crm-entity-history-list-rest-v3.md#item) and [crm.&lt;type&gt;.stagehistory.list](./crm-entity-stage-history-list-rest-v3.md#item) pages.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the item and permission to view history in a CRM access role

### Change History

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.history.list](./crm-entity-history-list-rest-v3.md) | Returns records about changes to the fields of an item or of all items of a type for a period ||
|| `crm.<type>.history.field.list` | Returns descriptions of history record fields ||
|| `crm.<type>.history.field.get` | Returns the description of a history record field by name ||
|#

### Stage History

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.stagehistory.list](./crm-entity-stage-history-list-rest-v3.md) | Returns transitions of an item or of all items of a type for a period through stages and pipelines ||
|| `crm.<type>.stagehistory.field.list` | Returns descriptions of stage history record fields ||
|| `crm.<type>.stagehistory.field.get` | Returns the description of a stage history record field by name ||
|#
