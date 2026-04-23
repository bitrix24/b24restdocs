# Additional Content Blocks for the Timeline: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Additional content blocks are customizable interface elements in the CRM timeline entry that the application adds alongside the main content of the entry.

These additional blocks are necessary when the standard entry view is insufficient, and there is a need to display application data directly within the CRM entity card without navigating to a separate interface. Such blocks can be used to show explanatory text, links, headings with nested blocks, and other elements in the timeline.

> Quick Navigation: [All Methods](#all-methods)

## Considerations Before Calling Methods

- Methods only work in the context of the [application](../../../../settings/app-installation/index.md).
- The application can only retrieve and remove the set of blocks that it has installed itself.
- When calling [crm.timeline.layout.blocks.set](./crm-timeline-layout-blocks-set.md) again, the previous set of blocks for the same application will be overwritten.

## Application Limitations

- Sets of additional content blocks cannot be installed for deals. For deals, use the methods [crm.activity.layout.blocks.*](../activities/layout-blocks/index.md).
- Sets of additional content blocks cannot be installed for [timeline log entries](../logmessage/index.md).
- Sets of additional content blocks cannot be installed for deprecated timeline entries.

## Lifecycle of Block Sets

- When a timeline entry is deleted, all sets of additional content blocks in that entry are permanently removed.
- When an application is deleted, all sets of additional content blocks that the application added to timeline entries are also deleted.

## How to Work with Additional Blocks

1. Prepare the description of the set of additional content blocks in the format of [RestAppLayoutDto](../activities/configurable/structure/rest-app-layout-dto.md).
2. Install the block set using the method [crm.timeline.layout.blocks.set](./crm-timeline-layout-blocks-set.md).
3. Retrieve the installed set using the method [crm.timeline.layout.blocks.get](./crm-timeline-layout-blocks-get.md).
4. Remove the set using the method [crm.timeline.layout.blocks.delete](./crm-timeline-layout-blocks-delete.md) if it is no longer needed.
5. If you need a ready-made workflow, check out the [test application example](./content-blocks-test-app.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

#| 
|| **Method** | **Description** ||
|| [crm.timeline.layout.blocks.set](./crm-timeline-layout-blocks-set.md) | Sets a set of additional content blocks in the timeline entry ||
|| [crm.timeline.layout.blocks.get](./crm-timeline-layout-blocks-get.md) | Retrieves the set of additional content blocks installed by the application for the timeline entry ||
|| [crm.timeline.layout.blocks.delete](./crm-timeline-layout-blocks-delete.md) | Deletes the set of additional content blocks installed by the application for the timeline entry ||
|#