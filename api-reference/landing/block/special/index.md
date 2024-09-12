# Menu Blocks

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet the writing standard.

{% endnote %}

{% endif %}

If you add the key **subtype** with the value `menu` in the block [manifest section](../manifest.md), the menu section will automatically populate with the sections of the current catalog when the block is added in the menu store.

In addition to subtype, the **block** section must contain the key **subtype_params** with the following parameters:

```php
'block' => array(
    'name' => 'Menu with logo on the left and menu items on the right',
    'section' => 'menu',
    'subtype' => 'menu',
    'subtype_params' => array(
        'selector' => '.landing-block-node-menu-list-item-link',
        'count' => 5,
        'source' => 'catalog',
    )
),
```
Where:
- **selector** - the selector for the menu item tag (tag a).
- **count** - limit on the number of dynamic items added (required, default is 5).
- **source** - data source:
  - catalog - menu of the current store catalog;
  - structure - structure of the current folders and pages, works only if the key menu is present in the [manifest](../manifest.md).

{% note warning %}

If the menu block is added to a regular landing page (not a store), this subtype will be ignored.

{% endnote %}