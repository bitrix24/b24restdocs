# Additional Content Blocks for an Activity: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Additional content blocks are customizable interface elements that the application adds to an activity alongside its main content in the CRM entity timeline.

These blocks are needed when the standard activity view is insufficient and application data has to be displayed right in the CRM entity card, without navigating to a separate interface. The application assembles a set of [content blocks](../configurable/structure/content-block.md) of six types:

- `text` — text
- `largeText` — long multiline text
- `link` — a link
- `deadline` — the activity deadline
- `withTitle` — a title with a nested block
- `lineOfBlocks` — several blocks in a single line

{% note info "" %}

The methods in this section only work in the context of the [application](../../../../../settings/app-installation/index.md). When called via a webhook, the methods return the `ERROR_WRONG_CONTEXT` error.

{% endnote %}

> Quick Navigation: [All Methods](#all-methods)

## Considerations Before Calling Methods

- The application can only retrieve and remove the set of blocks that it has installed itself. It neither sees nor modifies the sets of other applications.
- The restriction applies to the block set, not to the activity. The activity itself may have been created by another application or by an employee — only the current user's permissions for the CRM entity matter.
- When calling [crm.activity.layout.blocks.set](./crm-activity-layout-blocks-set.md) again, the previous set of blocks for the same application will be overwritten.
- A single set can contain no more than 20 blocks.
- The methods work only with activities. To add blocks to a comment or another timeline entry, use the [crm.timeline.layout.blocks.*](../../layout-blocks/index.md) methods.
- A set of blocks cannot be installed in an activity whose appearance is entirely defined by the [crm.activity.configurable.*](../configurable/index.md) methods.
- A set of blocks cannot be installed in an activity of a deprecated type — the timeline does not render such an activity as a configurable entry.
- The suitability of an activity cannot be determined in advance from REST data. The only way to check is a trial call: for an unsuitable activity, the `crm.activity.layout.blocks.set` method returns the `UNSUITABLE_ACTIVITY_TYPE_ERROR` error.

## Activity Linked to Multiple Entities

An activity can be linked to several CRM entities at once — for example, an e-mail can be linked to both a deal and a contact. A set of blocks added to such an activity is rendered in the timeline of every linked entity. The links are managed by the [crm.activity.binding.*](../binding/index.md) methods.

## Lifecycle of Block Sets

- When an activity is restored from the trash, the block sets added by applications are restored along with it.
- When an activity is permanently deleted from the trash, its block sets are removed for good.
- When an application is deleted, all block sets that it added to activities are permanently removed.

## How to Work with Additional Content Blocks

1. Find the activity: its identifier is returned by the [crm.activity.add](../activity-base/crm-activity-add.md) and [crm.activity.list](../activity-base/crm-activity-list.md) methods. The type and identifier of the CRM entity are passed in `entityTypeId` and `entityId`.
2. Prepare the description of the block set in the format of [RestAppLayoutDto](../configurable/structure/rest-app-layout-dto.md).
3. Install the set using the method [crm.activity.layout.blocks.set](./crm-activity-layout-blocks-set.md).
4. Retrieve the installed set using the method [crm.activity.layout.blocks.get](./crm-activity-layout-blocks-get.md).
5. Remove the set using the method [crm.activity.layout.blocks.delete](./crm-activity-layout-blocks-delete.md) if it is no longer needed.

A working scenario is covered in the [test application example](../../layout-blocks/content-blocks-test-app.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [crm.activity.layout.blocks.set](./crm-activity-layout-blocks-set.md) | Sets a collection of additional content blocks in an activity ||
|| [crm.activity.layout.blocks.get](./crm-activity-layout-blocks-get.md) | Retrieves the set of additional content blocks of an activity ||
|| [crm.activity.layout.blocks.delete](./crm-activity-layout-blocks-delete.md) | Deletes the set of additional content blocks from an activity ||
|#
