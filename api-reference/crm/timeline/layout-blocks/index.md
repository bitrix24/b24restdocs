# Additional Content Blocks

REST methods for working with additional content blocks in the timeline:

#|
|| **Method** | **Description** ||
|| [crm.timeline.layout.blocks.set](./crm-timeline-layout-blocks-set.md) | Sets a collection of additional content blocks in the timeline record ||
|| [crm.timeline.layout.blocks.get](./crm-timeline-layout-blocks-get.md) | Retrieves the set of additional content blocks established by the application for the timeline record ||
|| [crm.timeline.layout.blocks.delete](./crm-timeline-layout-blocks-delete.md) | Deletes the set of additional content blocks established by the application for the timeline record ||
|#

## Deleting Timeline Records

When a timeline record is deleted, the sets of additional content blocks added by applications will also be permanently removed.

## Deleting REST Application

When a REST application is deleted, all sets of additional content blocks added by it in the timeline records will be permanently removed.

## See Also

- [Example of a Test REST Application](./content-blocks-test-app.md)
- [REST methods for working with sets of additional content blocks for deals](../activities/layout-blocks/index.md)