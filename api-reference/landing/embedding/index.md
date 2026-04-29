# Embedding Locations in the Sites Section: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Embedding locations allow you to add an application to the interface of the "Sites" section. They define where in the interface the user will see the application or be able to invoke its action.

For example, an application can be added to the page settings to open it from the editor. Another option is to display an action in the block menu, allowing users to work with a specific block without navigating to another section.

> Quick navigation: [all methods](#all-methods)

## How to Work with Embedding Locations

In the "Sites" section, there are three main embedding options. Each uses its own embedding locations and methods.

**Site or Page Settings.** Use [LANDING_SETTINGS](./settings.md) if the application needs to open from the site or page settings. For instance, you can add an item in the page settings to check meta tags or to send the page for external moderation.

To configure:

1. Select the embedding location [LANDING_SETTINGS](./settings.md).
2. Register the embedding location using the internal method `landing.repo.bind`, not [placement.bind](../../widgets/placement-bind.md).
3. In the handler, parse `PLACEMENT_OPTIONS` to obtain the site and page identifiers.
4. If the embedding location is no longer needed, it can be removed using the method [landing.repo.unbind](./landing-repo-unbind.md).

**Actions with a Block.** Use [LANDING_BLOCK_*](./block.md) if the action needs to be available while editing a block. This option is suitable when the application works not with the entire page but with a specific block. For example, you can add an action for a text block that opens the translation interface, or an action for an image block that initiates a link check for a file.

To configure:

1. Select `LANDING_BLOCK_<CODE>` if the action is needed only for one type of block, or `LANDING_BLOCK_*` if the action should work for all blocks.
2. Register the embedding location using the internal method `landing.repo.bind`, not [placement.bind](../../widgets/placement-bind.md).
3. In the handler, parse `PLACEMENT_OPTIONS` to obtain the page, block, and block code identifiers.
4. If the embedding location is no longer needed, it can be removed using the method [landing.repo.unbind](./landing-repo-unbind.md).

**Linking the Knowledge Base to a Menu or Group.** Use the subsection [Embedding the Knowledge Base](./knowledge-base/index.md) if you need to display the Knowledge Base in a menu or link it to a group.

This option differs from other scenarios. Here, the internal method `landing.repo.bind` and [placement.bind](../../widgets/placement-bind.md) are not used; instead, separate binding methods are employed. This is because the Knowledge Base in the `landing` module is represented as a separate site.

To configure:

1. Obtain the Knowledge Base site identifier, for example, using the method [landing.site.getList](../site/landing-site-get-list.md).
2. Determine where to add the Knowledge Base: in the menu or to a group.
3. For the menu, use the methods [landing.site.bindingToMenu](./knowledge-base/landing-site-binding-to-menu.md), [landing.site.getMenuBindings](./knowledge-base/landing-site-get-menu-bindings.md), [landing.site.unbindingFromMenu](./knowledge-base/landing-site-unbinding-from-menu.md).
4. For the group, use the methods [landing.site.bindingToGroup](./knowledge-base/landing-site-binding-to-group.md), [landing.site.getGroupBindings](./knowledge-base/landing-site-get-group-bindings.md), [landing.site.unbindingFromGroup](./knowledge-base/landing-site-unbinding-from-group.md).

## Relationships with Other Sections

**Sites and Pages.** The embedding location [LANDING_SETTINGS](./settings.md) works in the editor for [sites](../site/index.md) and [pages](../page/index.md). Therefore, the site and page identifiers are passed to the handler.

**Blocks.** The embedding location [LANDING_BLOCK_*](./block.md) is associated with a specific block on the page. The block code, block identifier, and page identifier are used when working with the methods in the [Blocks](../block/index.md) and [Working with Blocks on the Page](../page/block-methods/index.md) sections.

**Widgets.** The general mechanics of embedding interfaces are described in the [Widgets](../../widgets/index.md) section, but for the `landing` module, registration is performed using the internal method `landing.repo.bind`, not [placement.bind](../../widgets/placement-bind.md).

**Knowledge Base.** Knowledge Base bindings use the methods of the [Site](../site/index.md) object because the Knowledge Base is represented as a separate site in the `landing` module.

## Overview of Embedding Locations and Methods {#all-methods}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

### Embedding Locations

#| 
|| **Embedding Location** | **When to Use** ||
|| [LANDING_SETTINGS](./settings.md) | Application item in the site or page settings menu ||
|| [LANDING_BLOCK_*](./block.md) | Application item in the editing actions of a specific block or all blocks ||
|| [Embedding the Knowledge Base](./knowledge-base/index.md) | Linking the Knowledge Base site to a menu or group ||
|#

### Managing Embedding Locations

#| 
|| **Method** | **Description** ||
|| [landing.repo.unbind](./landing-repo-unbind.md) | Removes the embedding location registered by the current application ||
|#

### Knowledge Base Methods

#| 
|| **Method** | **Description** ||
|| [landing.site.bindingToGroup](./knowledge-base/landing-site-binding-to-group.md) | Binds the Knowledge Base to a Social Network group ||
|| [landing.site.bindingToMenu](./knowledge-base/landing-site-binding-to-menu.md) | Binds the Knowledge Base to a menu ||
|| [landing.site.getGroupBindings](./knowledge-base/landing-site-get-group-bindings.md) | Retrieves Knowledge Base bindings to groups ||
|| [landing.site.getMenuBindings](./knowledge-base/landing-site-get-menu-bindings.md) | Retrieves Knowledge Base bindings to menus ||
|| [landing.site.unbindingFromGroup](./knowledge-base/landing-site-unbinding-from-group.md) | Unbinds the Knowledge Base from a Social Network group ||
|| [landing.site.unbindingFromMenu](./knowledge-base/landing-site-unbinding-from-menu.md) | Unbinds the Knowledge Base from a menu ||
|#