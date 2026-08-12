# Additional Content Blocks for an Activity: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Additional content blocks are customizable interface elements that the application adds to the activity card alongside the main content.

These blocks are needed when the standard activity view is insufficient and application data has to be displayed directly in the CRM entity timeline without navigating to a separate interface. With these blocks, you can show explanatory text, a link, a heading with a nested block, and other elements.

> Quick Navigation: [All Methods](#all-methods)

## Considerations Before Calling Methods

- Methods only work in the context of the [application](../../../../../settings/app-installation/index.md). When called via a webhook, the method returns the `ERROR_WRONG_CONTEXT` error.
- The application can only retrieve and remove the set of blocks that it has installed itself. It neither sees nor modifies the sets of other applications.
- The restriction applies to the block set, not to the activity. The activity itself may have been created by another application or by an employee — only the current user's permissions for the CRM entity matter.
- When calling [crm.activity.layout.blocks.set](./crm-activity-layout-blocks-set.md) again, the previous set of blocks for the same application will be overwritten.

## Usage Limitations

- Sets of blocks cannot be installed for a [configurable activity](../configurable/index.md): its appearance is entirely defined by the [crm.activity.configurable.*](../configurable/index.md) methods.
- Sets of blocks cannot be installed for an activity of a deprecated type.
- The methods work only with activities. To add blocks to a comment or another timeline entry, use the [crm.timeline.layout.blocks.*](../../layout-blocks/index.md) methods.

If the activity type is not suitable, the method returns the `UNSUITABLE_ACTIVITY_TYPE_ERROR` error.

## Activity Linked to Multiple Entities

An activity can be linked to several CRM entities at once — for example, an e-mail can be linked to both a deal and a contact. A set of blocks added to such an activity is rendered in the timeline of every linked entity. The links are managed by the [crm.activity.binding.*](../binding/index.md) methods.

## Lifecycle of Block Sets

- When an activity is restored from the trash, the block sets added by applications are restored along with it.
- When an application is deleted, all block sets that it added to activities are permanently removed.

## How to Work with Additional Blocks

1. Find the activity: its identifier is returned by the [crm.activity.add](../activity-base/crm-activity-add.md) and [crm.activity.list](../activity-base/crm-activity-list.md) methods. The type and identifier of the CRM entity are passed in `entityTypeId` and `entityId`.
2. Prepare the description of the block set in the format of [RestAppLayoutDto](../configurable/structure/rest-app-layout-dto.md).
3. Install the set using the method [crm.activity.layout.blocks.set](./crm-activity-layout-blocks-set.md).
4. Retrieve the installed set using the method [crm.activity.layout.blocks.get](./crm-activity-layout-blocks-get.md).
5. Remove the set using the method [crm.activity.layout.blocks.delete](./crm-activity-layout-blocks-delete.md) if it is no longer needed.
6. If you need a ready-made workflow, check out the [test application example](../../layout-blocks/content-blocks-test-app.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to read the CRM entity — for [crm.activity.layout.blocks.get](./crm-activity-layout-blocks-get.md), and a user with permission to modify the entity — for [crm.activity.layout.blocks.set](./crm-activity-layout-blocks-set.md) and [crm.activity.layout.blocks.delete](./crm-activity-layout-blocks-delete.md)

#|
|| **Method** | **Description** ||
|| [crm.activity.layout.blocks.set](./crm-activity-layout-blocks-set.md) | Sets a collection of additional content blocks in an activity ||
|| [crm.activity.layout.blocks.get](./crm-activity-layout-blocks-get.md) | Retrieves the set of additional content blocks established by the application in the activity ||
|| [crm.activity.layout.blocks.delete](./crm-activity-layout-blocks-delete.md) | Deletes the set of additional content blocks established by the application for the activity ||
|#

## See Also

- [{#T}](../../layout-blocks/index.md)
- [{#T}](../configurable/structure/content-block.md)
