# Working with Page Blocks: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The group of methods manages page blocks in edit mode. It allows you to add new blocks, change their order, hide, delete, restore, and save them to "My Blocks."

{% note warning "" %}

These methods modify blocks in the draft version of the page. If the page is already published, the block in the published version and the one in the edit version may have different identifiers. To make changes, pass the identifier of the block from the edit version. You can obtain it using the [landing.block.getList](../../block/methods/landing-block-get-list.md) method with the parameter `params.edit_mode = 1`. After making changes, publish the page using the [landing.landing.publication](../methods/landing-landing-publication.md) method.

{% endnote %}

> Quick Navigation: [All Methods](#all-methods)

## How to Work with Blocks

1. Obtain the page identifier using the [landing.landing.getList](../methods/landing-landing-get-list.md) method or use the `lid` returned by the [landing.landing.add](../methods/landing-landing-add.md), [landing.landing.addByTemplate](../methods/landing-landing-add-by-template.md), and [landing.landing.copy](../methods/landing-landing-copy.md) methods.
2. If you need to work with an existing block, retrieve the list of blocks using the [landing.block.getList](../../block/methods/landing-block-get-list.md) method with the parameter `params.edit_mode = 1`.
3. Choose the appropriate action based on your scenario:

   - [landing.landing.addblock](./landing-landing-add-block.md) — add a new block from the repository. Available block codes are returned by [landing.block.getrepository](../../block/methods/landing-block-get-repository.md).
   - [landing.landing.copyblock](./landing-landing-copy-block.md) — copy a block.
   - [landing.landing.moveblock](./landing-landing-move-block.md), [landing.landing.upblock](./landing-landing-up-block.md), [landing.landing.downblock](./landing-landing-down-block.md) — change the position of a block.
   - [landing.landing.hideblock](./landing-landing-hide-block.md) and [landing.landing.showblock](./landing-landing-show-block.md) — manage the visibility of a block.
   - [landing.landing.markdeletedblock](./landing-landing-mark-deleted-block.md), [landing.landing.markundeletedblock](./landing-landing-mark-undeleted-block.md), [landing.landing.deleteblock](./landing-landing-delete-block.md) — temporarily or permanently delete a block.
   - [landing.landing.favoriteBlock](./landing-landing-favorite-block.md) and [landing.landing.unFavoriteBlock](./landing-landing-unfavorite-block.md) — work with saved blocks.
4. After making changes, publish the page using the [landing.landing.publication](../methods/landing-landing-publication.md) method.

## Considerations

The `scope` parameter defines the internal type of the site or page. It is not related to the access permission `landing`. For pages in the `knowledge`, `group`, and `mainpage` scopes, provide the correct `scope`, otherwise the method may not find the page or block.

## How to Choose a Method

#| 
|| **If you need** | **Method** ||
|| Add a new block from the repository to the page | [landing.landing.addblock](./landing-landing-add-block.md) ||
|| Copy a block to the same or another page | [landing.landing.copyblock](./landing-landing-copy-block.md) ||
|| Move a block to another page | [landing.landing.moveblock](./landing-landing-move-block.md) ||
|| Change the order of a block on the page | [landing.landing.upblock](./landing-landing-up-block.md) or [landing.landing.downblock](./landing-landing-down-block.md) ||
|| Temporarily hide or show a block again | [landing.landing.hideblock](./landing-landing-hide-block.md) or [landing.landing.showblock](./landing-landing-show-block.md) ||
|| Mark a block as deleted or restore it | [landing.landing.markdeletedblock](./landing-landing-mark-deleted-block.md) or [landing.landing.markundeletedblock](./landing-landing-mark-undeleted-block.md) ||
|| Permanently delete a block from the page | [landing.landing.deleteblock](./landing-landing-delete-block.md) ||
|| Save a block to "My Blocks" or remove it from saved | [landing.landing.favoriteBlock](./landing-landing-favorite-block.md) or [landing.landing.unFavoriteBlock](./landing-landing-unfavorite-block.md) ||
|#

## Relationships with Other Objects

**List of Page Blocks.** The [landing.block.getList](../../block/methods/landing-block-get-list.md) method returns the composition of the page and the current state of its blocks.

**Block Repository.** The repository stores codes for standard blocks and blocks registered by the application. For an application block, use a code in the format `repo_<ID>`.

**Preview File.** For [landing.landing.favoriteBlock](./landing-landing-favorite-block.md), the identifier of the preview file is returned by [landing.block.uploadfile](../../block/methods/landing-block-upload-file.md).

## Overview of Methods {#all-methods}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the page

### Adding and Copying

#| 
|| **Method** | **Description** ||
|| [landing.landing.addblock](./landing-landing-add-block.md) | Adds a new block to the page ||
|| [landing.landing.copyblock](./landing-landing-copy-block.md) | Copies a block to the page ||
|| [landing.landing.favoriteBlock](./landing-landing-favorite-block.md) | Saves a block to "My Blocks" ||
|| [landing.landing.unFavoriteBlock](./landing-landing-unfavorite-block.md) | Removes a block from "My Blocks" ||
|#

### Position and Visibility

#| 
|| **Method** | **Description** ||
|| [landing.landing.moveblock](./landing-landing-move-block.md) | Moves a block on the page ||
|| [landing.landing.upblock](./landing-landing-up-block.md) | Moves a block up one position ||
|| [landing.landing.downblock](./landing-landing-down-block.md) | Moves a block down one position ||
|| [landing.landing.hideblock](./landing-landing-hide-block.md) | Hides a block on the page ||
|| [landing.landing.showblock](./landing-landing-show-block.md) | Shows a block on the page ||
|#

### Deletion and Restoration

#| 
|| **Method** | **Description** ||
|| [landing.landing.deleteblock](./landing-landing-delete-block.md) | Deletes a block from the page ||
|| [landing.landing.markdeletedblock](./landing-landing-mark-deleted-block.md) | Marks a block as deleted without physically removing it ||
|| [landing.landing.markundeletedblock](./landing-landing-mark-undeleted-block.md) | Restores a block from deleted status ||
|#