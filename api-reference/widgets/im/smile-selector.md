# Emoji Collection IM_SMILES_SELECTOR

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning " " %}

**DEPRECATED**

The `IM_SMILES_SELECTOR` embedding has been deprecated since version `im 25.1600.0`. Emojis have been replaced with [stickers](https://helpdesk.bitrix24.com/open/25866875/).

The [placement.bind](../placement-bind.md) method still accepts the placement code, but the handler is no longer called in the interface: the emoji selection window has been replaced with a new one, and it has no application tabs. This page is kept as an archival reference for older integrations.

{% endnote %}

> Scope: [`placement`](../../scopes/permissions.md)

The embedding used to add its own tab to the emoji selection window that opens from the message input field. In that tab, the application showed its own collection: custom emoji sets or a Giphy selection.

## What to Use Instead

Applications that need a button next to the message input field can use [{#T}](./textarea.md): this placement is active and receives the dialog identifier. The other messenger placements are collected in the [section overview](./index.md).

Stickers have no placement of their own, so an application cannot add its own sticker set to the interface.

## What Happens on Registration

Handler registration still goes through, and the placement remains visible in the [placement.get](../placement-get.md) response. This matters for older applications that registered a handler before emojis were replaced with stickers: their registration does not break the installation, but it does not display anything in the interface either.

For this placement, the [placement.bind](../placement-bind.md) method accepts the `OPTIONS` parameters `extranet`, `role`, and `context`, the same ones as [IM_CONTEXT_MENU](./context-menu.md). They have no effect on behavior, because the handler is not called.

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./textarea.md)
- [{#T}](./context-menu.md)
- [{#T}](../placement-bind.md)
- [{#T}](../placement-get.md)
- [{#T}](../placement-unbind.md)
