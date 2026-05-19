# Manifest File

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The manifest file accompanies each [block](./index.md) and describes:

- editable elements in the block markup
- styles available in the editor
- additional attributes
- connected resources

You can obtain the manifest file for a block using the method [landing.block.getmanifestfile](./methods/landing-block-get-manifest-file.md).

## Example of a Manifest File

```php
$manifest = [
    'block' => [
        'name' => 'Text and Image in Two Columns',
        'section' => ['text_image', 'columns'],
        'type' => ['page', 'store', 'smn', 'knowledge', 'group', 'mainpage'],
        'dynamic' => false,
        'description' => 'Block with a title, text, button, and image',
    ],
    'cards' => [
        '.landing-block-card' => [
            'name' => 'Column',
            'label' => ['.landing-block-node-title'],
        ],
    ],
    'nodes' => [
        '.landing-block-node-title' => [
            'name' => 'Title',
            'type' => 'text',
        ],
        '.landing-block-node-text' => [
            'name' => 'Text',
            'type' => 'text',
        ],
        '.landing-block-node-button' => [
            'name' => 'Button',
            'type' => 'link',
        ],
        '.landing-block-node-image' => [
            'name' => 'Image',
            'type' => 'img',
            'dimensions' => [
                'maxWidth' => 1200,
                'maxHeight' => 1200,
            ],
            'allowInlineEdit' => false,
            'useInDesigner' => true,
        ],
    ],
    'style' => [
        'block' => [
            'type' => ['block-default'],
        ],
        'nodes' => [
            '.landing-block-card' => [
                'name' => 'Column',
                'type' => ['columns', 'animation'],
            ],
            '.landing-block-node-title' => [
                'title' => 'Title',
                'type' => ['typo', 'heading'],
            ],
            '.landing-block-node-text' => [
                'title' => 'Text',
                'type' => 'typo',
            ],
            '.landing-block-node-button' => [
                'title' => 'Button',
                'type' => 'button',
            ],
            '.landing-block-node-image' => [
                'name' => 'Image',
                'type' => ['box'],
            ],
        ],
    ],
    'attrs' => [
        '.landing-block-node-text' => [
            [
                'name' => 'Display Mode',
                'type' => 'dropdown',
                'attribute' => 'data-view',
                'items' => [
                    ['name' => 'Short', 'value' => 'short'],
                    ['name' => 'Full', 'value' => 'full'],
                ],
            ],
        ],
    ],
    'assets' => [
        'css' => ['https://example.com/landing/custom-block.css'],
        'js' => ['https://example.com/landing/custom-block.js'],
        'ext' => ['landing_form'],
    ],
];
```

## Manifest Keys

The block manifest consists of a set of keys. Each key corresponds to a specific part of the block description: structure, editable elements, styles, attributes, and connected resources. Multiple keys can be used within a single manifest.

### Key block

The `block` key defines the basic properties of the block:

- `name` — the name of the block
- `section` — the section or array of sections in the block catalog. Current section codes can be obtained using the method [landing.block.getrepository](./methods/landing-block-get-repository.md)
- `dynamic` — indicates whether the block supports dynamic mode. If set to `false`, the block cannot be used as dynamic
- `subtype` — subtype of the special block, a single value or an array
- `subtype_params` — subtype parameters
- `type` — type of the site where the block is available. Supported site types: 
  - `page` — regular sites and landing pages
  - `store` — stores
  - `smn` — service type for the "Sites24" section in BUS
  - `knowledge` — knowledge bases
  - `group` — knowledge bases for social network groups
  - `mainpage` — main page of the account

Depending on the edition and version of the product, additional service values for `type` may be present.

### Key nodes

The `nodes` key describes elements that can be edited as content. CSS selectors are used to specify nodes. It is recommended to choose clear structural classes as selectors, for example, with the prefix `landing-block-node-`.

The same selector can be used in different blocks, but the node selector must not match the card selector of the same block.

In `nodes`, the keys are the selectors of editable elements, and the values specify the node label, its type, and additional parameters. The type of the node determines how the element will be edited in the interface.

Possible node types:

- `text` — text content of the element
- `img` — image 
- `link` — link 
- `icon` — icon
- `embed` — embedded media content
- `map` — map
- `component` — embedded component
- `styleimg` — image controlled through style settings

More details: [Node Types](./node-types.md).

### Key cards

The `cards` key describes cards. Cards are used for repeatable content, such as lists of services, employees, or gallery items.

In the basic version, it is sufficient to describe cards in the manifest, while more complex scenarios use advanced card management.

Recommendations:

- use separate selectors for cards and nodes
- use clear structural classes, such as `landing-block-card-*`
- do not mix cards of different selectors in one common parent without an advanced card scheme

More about the advanced scheme: [Extended Card Description](./extended-description.md).

### Key style

The `style` key defines which style settings are available in the editor.

When changing the appearance of blocks, CSS classes are usually modified rather than the inline `style` attribute of nodes. For example, when changing text size, the system may replace the conditional class `g-font-size-12` with `g-font-size-16`, rather than directly writing `font-size` into `style`.

Structure:

- `style.block` — styles for the entire block
- `style.nodes` — styles for individual elements within the block by CSS selectors

Style types:

- `block-default` — basic styling settings for the block
- `block-default-background` — basic block with a background
- `block-default-background-height-vh` — block with a background and viewport height
- `paddings` — outer and inner paddings
- `margins` — outer margins
- `box` — container settings (color, shadow, opacity)
- `bg` — background color
- `button` — button styles
- `typo` — advanced typographic settings for text
- `typo-simple` — simplified typography settings
- `typo-link` — link styling
- `navbar` — navigation bar settings
- `navbar-bg-color` — navigation bar settings with background
- `navbar-full` — advanced navigation bar settings
- `block-default-background-overlay` — basic block with background overlay settings
- `block-default-background-overlay-height-vh` — variant with overlay and screen height setting
- `block-default-wo-background` — basic block without background settings
- `block-default-wo-paddings` — block without padding settings
- `block-default-wo-background-vh-animation` — block without background, with screen height setting and animation
- `block-border` — block border settings
- `container` — content container settings
- `heading` — heading settings
- `border-colors` — border colors and hover border colors
- `background-gradient` — gradient background
- `background-hover` — hover background
- `widget` — styles for widgets

The set of style types and parameters within each type is extensible and depends on the connected style manifests and product version.

{% cut "Reference List of Style Types and Their Parameters" %}

```php
$styleTypes = [
    'block-default' => [
        'display',
        'background',
        'padding-top',
        'padding-bottom',
        'padding-left',
        'padding-right',
        'margin-top',
    ],
    'block-default-background' => [
        'display',
        'background',
        'padding-top',
        'padding-bottom',
        'padding-left',
        'padding-right',
        'margin-top',
    ],
    'block-default-background-height-vh' => [
        'display',
        'background',
        'height-vh',
        'padding-top',
        'padding-bottom',
        'padding-left',
        'padding-right',
        'margin-top',
    ],
    'paddings' => [
        'padding-top',
        'padding-bottom',
        'padding-left',
        'padding-right',
    ],
    'margins' => [
        'margin-top',
        'margin-bottom',
        'margin-left',
        'margin-right',
    ],
    'box' => [
        'background-color',
        'box-shadow',
        'opacity',
    ],
    'bg' => [
        'background-color',
    ],
    'button' => [
        'button-color',
        'button-color-hover',
        'button-type',
        'button-size',
        'button-padding',
        'border-radius',
        'color',
        'color-hover',
        'border-color-hover',
        'font-family',
        'text-transform',
    ],
    'typo' => [
        'text-align',
        'color',
        'font-size',
        'font-family',
        'font-weight',
        'text-decoration',
        'text-transform',
        'line-height',
        'letter-spacing',
        'word-break',
        'text-shadow',
        'padding-top',
        'padding-left',
        'padding-right',
        'margin-bottom',
    ],
    'typo-simple' => [
        'font-size',
        'font-family',
        'font-weight',
        'text-decoration',
        'text-transform',
        'line-height',
        'letter-spacing',
    ],
    'typo-link' => [
        'color',
        'color-hover',
        'font-size',
        'font-family',
        'font-weight',
        'text-decoration',
        'text-transform',
        'letter-spacing',
        'text-shadow',
    ],
    'navbar' => [
        'navbar-align',
        'navbar-color',
        'navbar-color-hover',
    ],
    'navbar-bg-color' => [
        'navbar-align',
        'navbar-color',
        'navbar-bg',
        'navbar-color-hover',
        'navbar-bg-hover',
    ],
    'navbar-full' => [
        'navbar-align',
        'navbar-color',
        'navbar-color-hover',
        'navbar-color-fix-moment',
        'navbar-color-fix-moment-hover',
    ],
    'block-default-background-overlay' => [
        'display',
        'background-attachment',
        'background-size',
        'padding-top',
        'padding-bottom',
        'padding-left',
        'padding-right',
        'margin-top',
        'background-overlay',
    ],
    'block-default-background-overlay-height-vh' => [
        'display',
        'background-attachment',
        'background-size',
        'height-vh',
        'padding-top',
        'padding-bottom',
        'padding-left',
        'padding-right',
        'margin-top',
        'background-overlay',
    ],
    'block-default-wo-background' => [
        'display',
        'padding-top',
        'padding-bottom',
        'padding-left',
        'padding-right',
        'margin-top',
    ],
    'block-default-wo-paddings' => [
        'display',
        'background-color',
    ],
    'block-default-wo-background-vh-animation' => [
        'display',
        'padding-top',
        'padding-bottom',
        'padding-left',
        'padding-right',
        'margin-top',
        'height-vh',
        'animation',
    ],
    'block-border' => [
        'background',
        'block-border-type',
        'block-border-margin',
        'border-radius',
        'block-border-position',
    ],
    'container' => [
        'container-max-width',
        'padding-left',
        'padding-right',
    ],
    'heading' => [
        'text-align',
        'heading-v2',
        'border-color',
        'border-color-hover',
        'margin-bottom',
    ],
    'border-colors' => [
        'border-color',
        'border-color-hover',
    ],
    'background-gradient' => [
        'background-color',
    ],
    'background-hover' => [
        'background-color-hover',
    ],
    'widget' => [
        'background',
        'widget-type',
        'margin-top',
        'margin-bottom',
        'padding-top',
        'padding-bottom',
        'padding-left',
        'padding-right',
    ],
];
```

{% endcut %}

### Key attrs

The `attrs` key describes additional parameters, the values of which are stored in the attributes of DOM elements.

Attribute types:

- `text` — single-line text field
- `date` — date and time selection
- `html` — multi-line text field
- `dropdown` — dropdown list
- `checkbox` — checkbox or group of checkboxes
- `radio` — single choice from a list
- `multiselect` — multiple selection
- `image` — image selection
- `icon` — icon selection
- `link` — link with extended settings
- `url` — simplified URL field
- `slider` — single value slider
- `range-slider` — range value slider
- `palette` — selection from a palette
- `color` — color selection
- `sortable-list` — sortable list of values
- `position` — selection of the position/direction of the element
- `catalog-view` — settings for displaying catalog data
- `filter` — filter settings
- `user-select` — user selection
- `dynamic_source` — selection of a dynamic data source

More details: [Attributes](./attributes.md).

### Keys lang_original and lang

The `lang_original` and `lang` keys define the localization of labels in the block manifest.

- `lang_original` — the original language of the phrases in the manifest
- `lang` — a set of translations by language

Recommendations:

- set `lang_original` according to the actual language of the manifest
- use the same phrase keys in `lang` as in the original manifest

More details: [Block Localization](./localization.md).

### Key menu

The `menu` key is used when a multi-level menu with separate settings for root and child items is needed.

If only one level is sufficient, the menu can also be implemented as regular cards with links.

Example of a multi-level menu:

```php
'menu' => [
    '.landing-block-node-menu' => [
        'item' => '.landing-block-node-menu-item',
        'name' => 'Menu',
        'root' => [
            'ulClassName' => 'landing-block-node-menu navbar-nav',
            'liClassName' => 'landing-block-node-menu-item nav-item',
            'aClassName' => 'landing-block-node-menu-link nav-link',
        ],
        'children' => [
            'ulClassName' => 'landing-block-node-menu navbar-nav',
            'liClassName' => 'landing-block-node-menu-item nav-item',
            'aClassName' => 'landing-block-node-menu-link nav-link',
        ],
        'nodes' => [
            '.landing-block-node-menu-link' => [
                'name' => 'Link',
                'type' => 'link',
            ],
        ],
    ],
]
```

Main fields:

- the key of the array `.landing-block-node-menu` — selector for the root `<ul>`
- `item` — selector for `<li>` elements
- `name` — name of the menu in the interface
- `root` — classes for the root level of the menu: 
  - `ulClassName` for the `<ul>` container
  - `liClassName` for `<li>` items
  - `aClassName` for `<a>` links
- `children` — classes for child levels of the menu:
    - `ulClassName` for nested `<ul>`
    - `liClassName` for nested `<li>`,
    - `aClassName` for `<a>` links in child items
- `nodes` — editable elements within the menu item, such as a link

Multiple multi-level menus can be described in a single manifest, meaning multiple root selectors in `menu`.

### Key assets

The `assets` key defines the JS and CSS resources that are connected when adding a block to the page.

- `css` — external CSS files
- `js` — external JS files
- `ext` — core Bitrix JS extensions

If the same file is already included by another block, it will not be added again.

The `ext` key is used to connect core libraries. Only supported extensions available in the block environment should be connected.

Supported:

- `landing_form` — logic and interfaces for form blocks
- `landing_carousel` — logic for carousel/slider for cards and images
- `landing_google_maps_new` — integration with Google Maps in the current scenario
- `landing_map` — basic logic for maps in blocks
- `landing_countdown` — countdown timer
- `landing_gallery_cards` — gallery of cards/images
- `landing_chat` — chat scenarios in landings

For repository blocks, the extension for Vue widgets `landing.widgetvue` is additionally used.

If the script uses libraries loaded by the core, it is better to wrap the initialization in `BX.ready(...)` to ensure the code runs after system connections:

```js
BX.ready(function () {
    console.log($(window));
});
```

## Animation

For the animation to work properly:

- the node must have the class `js-animation`
- in `style`, the type `animation` must be specified for this node
- if necessary, you can immediately add an effect class, for example, `fadeIn`