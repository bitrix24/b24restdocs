# List of Methods for Working with Blocks

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

Adding a block occurs in the page editing mode. In this mode, the block identifiers differ from those on the published page. After working with blocks, the page must be published.

#|
|| **Method** | **Description** | **Version** ||
|| [landing.landing.addblock](./landing-landing-add-block.md) | Method for adding a new block to the page. | ||
|| [landing.landing.copyblock](./landing-landing-copy-block.md) | Method for copying a block from one page to another. | ||
|| [landing.landing.deleteblock](./landing-landing-delete-block.md) | Method for deleting a block from the page. | ||
|| [landing.landing.downblock](./landing-landing-down-block.md) | Method for moving a block down one position on the page. | ||
|| [landing.landing.favoriteBlock](./landing-landing-favorite-block.md) | Method saves an existing block on the page to "My Blocks." | 21.800.0 ||
|| [landing.landing.hideblock](./landing-landing-hide-block.md) | Method hides a block from the page. | ||
|| [landing.landing.markdeletedblock](./landing-landing-mark-deleted-block.md) | Method marks a block as deleted but does not physically remove it. | ||
|| [landing.landing.markundeletedblock](./landing-landing-mark-undeleted-block.md) | Method restores a block from those marked as deleted. | ||
|| [landing.landing.moveblock](./landing-landing-move-block.md) | Method for moving a block from one page to another. | ||
|| [landing.landing.showblock](./landing-landing-show-block.md) | Method displays a block on the page. | ||
|| [landing.landing.unFavoriteBlock](./landing-landing-unfavorite-block.md) | Method removes a block that was saved in "My Blocks." | 21.800.0 ||
|| [landing.landing.upblock](./landing-landing-up-block.md) | Method for moving a block up one position on the page. | ||
|#