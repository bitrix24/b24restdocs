# Additional Content Blocks

REST methods for working with additional content blocks in a deal:

#|
|| **Method** | **Description** ||
|| [crm.activity.layout.blocks.set](./crm-activity-layout-blocks-set.md) | This method allows REST applications to set a set of additional content blocks in a deal. ||
|| [crm.activity.layout.blocks.get](./crm-activity-layout-blocks-get.md) | This method allows REST applications to retrieve the set of additional content blocks that they have set in the deal. ||
|| [crm.activity.layout.blocks.delete](./crm-activity-layout-blocks-delete.md) | This method allows REST applications to delete the set of additional content blocks that they have set for the deal. ||
|#

## Deal Linked to Multiple Entities

Adding sets of additional content blocks to a deal linked to multiple entities will result in the sets of additional content blocks being rendered in each of the deals within the timeline.

## Restoring Deals from Trash

When restoring deals from the trash, the sets of additional content blocks added by applications will also be restored.

## Deleting REST Application

When a REST application is deleted, all sets of additional content blocks added by it to the deals will be permanently removed.

## See Also

- [Example of a Test REST Application](../../layout-blocks/content-blocks-test-app.md)
- [REST Methods for Working with Sets of Additional Content Blocks in the Timeline](../../layout-blocks/index.md)