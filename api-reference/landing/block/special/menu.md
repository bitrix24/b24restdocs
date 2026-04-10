# Menu Blocks

Menu blocks are components that can be automatically populated with links. This behavior is enabled through `subtype: menu` in the `block` section of the [block manifest](../manifest.md).

For blocks of this type in `landing`, two data sources are used:

- `catalog` for the store catalog menu
- `personal` for the personal account menu

## Catalog Menu

This is a menu block variant for the store catalog. When adding the block, the system populates it with menu items from the current catalog.

Example manifest:

```php
'block' => array(
    'name' => 'Menu with logo on the left and menu items on the right',
    'section' => 'menu',
    'subtype' => 'menu',
    'subtype_params' => array(
        'selector' => '.landing-block-node-menu-list-item-link',
        'count' => 5,
        'source' => 'catalog',
    ),
),
```

Where:

- **selector** - the link selector where the menu items are inserted
- **count** - the limit on the number of automatically added items
- **source** - for the catalog menu, use the value `catalog`

In standard blocks, the `selector` parameter points to the link where the menu items are inserted. For example, this could be `.landing-block-node-menu-list-item-link` or `.landing-block-node-list-item`. The `count` parameter is not present in all blocks.

## Personal Account Menu

If the `source` is set to `personal`, the system will insert a predefined set of links for the personal account. Typically, this menu includes links to the main sections of the personal account, such as orders, profile, and cart.

## Important Considerations

- `source: catalog` is suitable for store and catalog blocks
- `source: personal` is suitable for personal account menus
- The set of parameters depends on the specific block's markup. The `selector` is usually used for automatic population
- In standard examples, the block also has a `dynamic` parameter set to `false`
- In standard blocks for menus, the `landing_menu` extension is often included, and in some headers, `landing_header` is additionally used

{% note warning "" %}

For a block with `source: catalog`, the corresponding store or catalog context is required. If such context is not present, automatic menu population may not work.

{% endnote %}

To ensure the menu block is populated automatically, check the data source and block markup in advance. For `catalog` and `personal`, a correct `selector` is usually needed.

## Continue Your Learning

- [Special Blocks](./index.md)
- [Maps in Blocks](./maps.md)
- [Navigation and Header](./navigation.md)
- [Search Results](./search.md)
- [Search Forms](./search-forms.md)
- [Forms in Blocks](./crm-forms.md)