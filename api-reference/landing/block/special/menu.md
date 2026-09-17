# Menu Blocks

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A menu block is populated with links automatically. Instead of a manual list of items, the block receives them from Bitrix24: the sections of the online store catalog or the links of the personal account. This behavior is enabled through `subtype: menu` in the `block` section of the [block manifest](../manifest.md).

The scenario suits online stores with a changing catalog structure and the pages of a customer's personal account. If the menu items are constant, the subtype is not needed: the links are described with regular [cards](../extended-description.md) or with the [`menu` key](../manifest.md#menu-key) of the manifest, which is responsible for a multi-level menu with manual items.

The source of the links is set in the `subtype_params.source` parameter: [Catalog Menu](#catalog-menu) or [Personal Account Menu](#personal-menu).

## Subtype Parameters

#|
|| **Parameter** | **Default Value** | **What It Defines** ||
|| `source` | — | The source of the links: `catalog` or `personal` ||
|| `selector` | `.landing-block-node-menu-list-item-link` | The selector of the menu item link. The system writes the substituted items into the nodes matching this selector, so the selector has to be described in the `nodes` key of the manifest ||
|| `count` | `5` | The maximum number of automatically added items. Applies only to `source: catalog`: the set of personal account links is fixed ||
|| `navbarCollapseSection` | `.navbar-collapse` | The selector of the collapsible part of the navigation ||
|| `navbarTogglerButton` | `button.navbar-toggler` | The selector of the button that expands the menu ||
|#

What matters about `selector`:

- the selector has to be described in the [`nodes` key](../manifest.md#nodes-key) of the manifest. Otherwise the subtype finds nowhere to write and the items are not substituted
- standard blocks use three values: `.landing-block-node-menu-list-item-link` in header blocks with a horizontal menu, `.landing-block-node-list-item` in blocks with a list of links, and `.landing-node-item-link` in personal account blocks

Extensions are not required for substituting the links, but they are responsible for the behavior of the menu itself on the page, so they are connected in `assets.ext`:

- `landing_menu` — in all menu blocks
- `landing_header` — additionally in header blocks

## What the Subtype Does When a Block Is Added

The subtype does not append items to the existing ones, it replaces them: it invokes the node update mechanism and writes its own set in full. For each item, the link text and the address are set; for catalog items, the `data-url` attribute with the same address is set as well.

Besides the links, the subtype also prepares the collapsible menu: it describes hidden attributes for `.navbar-collapse` and for the `button.navbar-toggler` button in the manifest, and when the block is added it writes an identifier of the `navBar<block ID>` form into them: the `id` attribute into `.navbar-collapse`, and `aria-controls` with the same value plus `data-target` with the value prefixed by a hash into the button. This keeps two menus on the same page from conflicting.

## Catalog Menu: `source: catalog` {#catalog-menu}

When the block is added, the system substitutes the sections of the current store catalog into it.

Example manifest:

```php
'block' => [
    'name' => 'Menu with logo on the left and menu items on the right',
    'section' => 'menu',
    'subtype' => 'menu',
    'subtype_params' => [
        'selector' => '.landing-block-node-menu-list-item-link',
        'count' => 5,
        'source' => 'catalog',
    ],
],
'nodes' => [
    '.landing-block-node-menu-list-item-link' => [
        'name' => 'Menu item',
        'type' => 'link',
    ],
],
'assets' => [
    'ext' => ['landing_menu'],
],
```

Example markup of a menu item:

```html
<ul class="landing-block-node-menu-list">
    <li>
        <a class="landing-block-node-menu-list-item-link" href="#">Catalog</a>
    </li>
</ul>
```

The catalog menu is populated only on a site of the `store` type, and it takes the sections of the catalog selected in the site settings. Without that context, the items are not substituted and the block keeps its original markup.

## Personal Account Menu: `source: personal` {#personal-menu}

If `source` is set to `personal`, the system substitutes a ready-made set of personal account links:

- Personal area
- Current orders
- Personal account
- Personal information
- Order history
- Order profiles
- Shopping cart
- Subscription

The set is fixed: it depends neither on the block markup nor on the catalog, and the `count` parameter does not affect it. The manifest is described in the same way as for the catalog, except that `source` is changed to `personal` and `count` is not set.

## How to Check a Menu Block

1. Find a suitable standard block using the [landing.block.getrepository](../methods/landing-block-get-repository.md) method in the `menu` section.
2. Review its manifest using the [landing.block.getmanifestfile](../methods/landing-block-get-manifest-file.md) method and compare `subtype_params` with your own block.
3. Register your block using the [landing.repo.register](../../user-blocks/landing-repo-register.md) method and add it to a page using the [landing.landing.addblock](../../page/block-methods/landing-landing-add-block.md) method.
4. Check the result using the [landing.block.getcontent](../methods/landing-block-get-content.md) method with the `editMode = true` parameter: the substituted links have to appear in the markup. Without `editMode`, the method returns the published version of the block, where the changes are not present yet.

## Permissions and Limitations

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

Limitations:

- there are two data sources: `catalog` and `personal`. Standard blocks also contain a third `source` value — `structure`, but in the current version of the module it substitutes nothing. An arbitrary source cannot be connected in `subtype_params`
- the items are substituted once, when the block is added to a page. If the catalog changes, the menu does not rebuild itself — update the links using the [landing.block.updatenodes](../methods/landing-block-update-nodes.md) method

## Continue Your Learning

- [{#T}](./index.md)
- [{#T}](./maps.md)
- [{#T}](./navigation.md)
- [{#T}](./search.md)
- [{#T}](./search-forms.md)
- [{#T}](./crm-forms.md)