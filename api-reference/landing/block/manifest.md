# Manifest File

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A manifest is the description of a [block](./index.md) in the form of an array. The block markup defines how it looks, while the manifest defines what can be changed in it:

- editable elements in the block markup
- styles available in the editor
- additional attributes
- connected resources

For standard Bitrix24 blocks, the manifest is located in the `.description.php` file next to the block. An app passes the manifest of its own block at registration — in the `manifest` parameter of the [landing.repo.register](../user-blocks/landing-repo-register.md) method.

The examples on this page are written as a PHP array, as in `.description.php`. In a REST call, the same set of keys is passed as a JSON object: the `manifest` parameter is declared with the `object` type.

The manifest is needed for two tasks: when you build your own block and when you modify someone else's block via REST. In the second case, the manifest shows which selectors the modification methods accept: nodes from `nodes`, cards from `cards`, attributes from `attrs`, and styles from `style`.

## How to Retrieve the Manifest

- [landing.block.getmanifestfile](./methods/landing-block-get-manifest-file.md) — the original manifest of a template by block code, `01.big_with_text` for example
- [landing.block.getmanifest](./methods/landing-block-get-manifest.md) — the manifest of a block already placed on a page, with service fields and substituted translations

Examine a manifest on a standard block: retrieve the list of templates using the [landing.block.getrepository](./methods/landing-block-get-repository.md) method, and then request the manifest of the required block by its code. Call parameters for `landing.block.getmanifestfile`:

```json
{
    "code": "01.big_with_text"
}
```

A manifest does not always have to be written. If you take a ready-made Bitrix24 block and change only the values of its nodes and attributes, go straight to the methods of the [Working with Blocks](./methods/index.md) section.

## Example of a Manifest File

```php
$manifest = [
    'block' => [
        'name' => 'Text and Image in Two Columns',
        'section' => ['text_image', 'columns'],
        'type' => ['page', 'store', 'knowledge', 'group'],
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
                'name' => 'Title',
                'type' => ['typo', 'heading'],
            ],
            '.landing-block-node-text' => [
                'name' => 'Text',
                'type' => 'typo',
            ],
            '.landing-block-node-button' => [
                'name' => 'Button',
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

The block manifest consists of a set of keys. Each key corresponds to a specific part of the block description. Multiple keys are used within a single manifest.

The manifest itself is an optional parameter of the [landing.repo.register](../user-blocks/landing-repo-register.md) method: without it the block will be registered, but there will be nothing to edit in it. The minimum working set for your own block is `nodes` with the editable elements. The other keys are added as needed.

#|
|| **Key** | **What It Describes** | **Details** ||
|| `block` | The block name, the catalog section, the site types, and the special block subtype | [Key block](#block-key) ||
|| `nodes` | The editable elements of the block and their types | [Key nodes](#nodes-key), [Node Types](./node-types.md) ||
|| `cards` | Repeatable elements: cards of services, employees, or slides | [Key cards](#cards-key), [Extended Description of Cards](./extended-description.md) ||
|| `style` | The style settings available in the editor | [Key style](#style-key) ||
|| `attrs` | Additional settings retained in DOM attributes | [Key attrs](#attrs-key), [Attributes](./attributes.md) ||
|| `menu` | A multi-level menu with settings for root and child items | [Key menu](#menu-key) ||
|| `assets` | Connected CSS files, JS files, and core extensions | [Key assets](#assets-key) ||
|| `lang_original`, `lang` | The original language of the labels and their translations | [Keys lang_original and lang](#lang-key), [Block Localization](./localization.md) ||
|#

### Key block {#block-key}

The `block` key defines the basic properties of the block. For a block that an app registers with the [landing.repo.register](../user-blocks/landing-repo-register.md) method, the name, the description, and the catalog sections are taken from the `fields.NAME`, `fields.DESCRIPTION`, and `fields.SECTIONS` fields rather than from here: for such a block, only `type`, `subtype`, and `subtype_params` are read from the `block` key.

Fields of the key:

- `name` — the name of the block
- `section` — the section or array of sections in the block catalog. Current section codes can be obtained using the method [landing.block.getrepository](./methods/landing-block-get-repository.md)
- `dynamic` — indicates whether a standard block supports dynamic mode. By default the key is not set, and the block can be used as dynamic. Only an explicit `false` value prohibits the mode
- `subtype` — subtype of the special block, a single value or an array. If there are several values, the handlers are applied in turn and each one extends the manifest. Which subtypes exist: [Special Blocks](./special/index.md)
- `subtype_params` — subtype parameters. Each scenario has its own: they are listed on the scenario page in the [Special Blocks](./special/index.md) section
- `type` — type of the site where the block is available. Supported site types:
  - `page` — regular sites and landing pages
  - `store` — stores
  - `smn` — service type for the "Sites24" section in BUS
  - `knowledge` — knowledge bases
  - `group` — knowledge bases for social network groups
  - `vibe` — the Bitrix24 main page

The `page` value automatically adds the `smn` type to the block as well. If the `type` key is not set, the block is considered common and will be available in all site types. To remove a block from the catalog, pass the string `null` or an empty string in `type` — such a block is hidden.

### Key nodes {#nodes-key}

The `nodes` key describes elements that can be edited as content. CSS selectors are used to specify nodes. It is recommended to choose clear structural classes as selectors, for example, with the prefix `landing-block-node-`.

The same selector can be used in different blocks. However, it is better to avoid a node selector matching a card selector within the same block: the system does not check this at registration, but in the editor it becomes unclear what exactly is being edited.

In `nodes`, the keys are the selectors of editable elements, and the values specify the node label, its type, and additional parameters. The type of the node determines how the element will be edited in the interface and in which format its value is retained.

There are eight main node types, from text and images to maps and embedded components. The list of types, their fields, and markup examples: [Node Types](./node-types.md).

### Key cards {#cards-key}

The `cards` key describes cards. Cards are used for repeatable content, such as lists of services, employees, or gallery items.

In `cards`, the keys are the selectors of repeatable elements. The basic description fields are:

- `name` — the name of the card in the settings form
- `label` — a node selector or an array of selectors the card title in the list is assembled from

The other fields — presets, grouping, and action restrictions — belong to the extended scheme and are covered in the [Extended Description of Cards](./extended-description.md) article.

Recommendations:

- use separate selectors for cards and nodes
- use clear structural classes, such as `landing-block-card-*`
- do not mix cards of different selectors in one common parent without an advanced card scheme

If the cards are menu items and separate settings are needed for the root and child levels, use the [key menu](#menu-key) instead of `cards`.

### Key style {#style-key}

The `style` key defines which style settings are available in the editor.

When changing the appearance of blocks, CSS classes are usually modified rather than the inline `style` attribute of nodes. For example, when changing text size, the system may replace the conditional class `g-font-size-12` with `g-font-size-16`, rather than directly writing `font-size` into `style`.

Structure:

- `style.block` — styles for the entire block
- `style.nodes` — styles for individual elements within the block by CSS selectors

The style groups and the parameters each of them opens in the editor are listed below. An individual parameter code can also be passed in `type`, `columns`, `animation`, `display`, `background`, or `border-radius` for example.

#|
|| **Group** | **What It Configures** | **Parameters** ||
|| `block-default` | The basic design of the block | `display`, `background`, `padding-top`, `padding-bottom`, `padding-left`, `padding-right`, `margin-top` ||
|| `block-default-background` | A basic block with a background | `display`, `background`, `padding-top`, `padding-bottom`, `padding-left`, `padding-right`, `margin-top` ||
|| `block-default-background-height-vh` | A block with a background and viewport height | `display`, `background`, `height-vh`, `padding-top`, `padding-bottom`, `padding-left`, `padding-right`, `margin-top` ||
|| `block-default-background-overlay` | A basic block with a background overlay | `display`, `background-attachment`, `background-size`, `padding-top`, `padding-bottom`, `padding-left`, `padding-right`, `margin-top`, `background-overlay` ||
|| `block-default-background-overlay-height-vh` | An overlay together with the screen height setting | `display`, `background-attachment`, `background-size`, `height-vh`, `padding-top`, `padding-bottom`, `padding-left`, `padding-right`, `margin-top`, `background-overlay` ||
|| `block-default-wo-background` | A basic block without background settings | `display`, `padding-top`, `padding-bottom`, `padding-left`, `padding-right`, `margin-top` ||
|| `block-default-wo-paddings` | A block without padding settings | `display`, `background-color` ||
|| `block-default-wo-background-vh-animation` | A block without a background, with screen height and animation | `display`, `padding-top`, `padding-bottom`, `padding-left`, `padding-right`, `margin-top`, `height-vh`, `animation` ||
|| `block-border` | The border of the block | `background`, `block-border-type`, `block-border-margin`, `border-radius`, `block-border-position` ||
|| `paddings` | Inner paddings | `padding-top`, `padding-bottom`, `padding-left`, `padding-right` ||
|| `margins` | Outer margins | `margin-top`, `margin-bottom`, `margin-left`, `margin-right` ||
|| `container` | The content container | `container-max-width`, `padding-left`, `padding-right` ||
|| `box` | The color, shadow, and opacity of the container | `background-color`, `box-shadow`, `opacity` ||
|| `bg` | The background color | `background-color` ||
|| `background-gradient` | A gradient background | `background-color` ||
|| `background-hover` | The background on hover | `background-color-hover` ||
|| `border-colors` | The border color and the border color on hover | `border-color`, `border-color-hover` ||
|| `button` | The design of a button | `button-color`, `button-color-hover`, `button-type`, `button-size`, `button-padding`, `border-radius`, `color`, `color-hover`, `border-color-hover`, `font-family`, `text-transform` ||
|| `heading` | A heading | `text-align`, `heading-v2`, `border-color`, `border-color-hover`, `margin-bottom` ||
|| `typo` | Advanced text typography | `text-align`, `color`, `font-size`, `font-family`, `font-weight`, `text-decoration`, `text-transform`, `line-height`, `letter-spacing`, `word-break`, `text-shadow`, `padding-top`, `padding-left`, `padding-right`, `margin-bottom` ||
|| `typo-simple` | Simplified typography | `font-size`, `font-family`, `font-weight`, `text-decoration`, `text-transform`, `line-height`, `letter-spacing` ||
|| `typo-link` | The design of links | `color`, `color-hover`, `font-size`, `font-family`, `font-weight`, `text-decoration`, `text-transform`, `letter-spacing`, `text-shadow` ||
|| `navbar` | The navigation bar | `navbar-align`, `navbar-color`, `navbar-color-hover` ||
|| `navbar-bg-color` | The navigation bar with a background | `navbar-align`, `navbar-color`, `navbar-bg`, `navbar-color-hover`, `navbar-bg-hover` ||
|| `navbar-full` | The navigation bar with settings for the pinned state | `navbar-align`, `navbar-color`, `navbar-color-hover`, `navbar-color-fix-moment`, `navbar-color-fix-moment-hover` ||
|| `widget` | The design of widgets | `background`, `widget-type`, `margin-top`, `margin-bottom`, `padding-top`, `padding-bottom`, `padding-left`, `padding-right` ||
|#

The set of groups and the parameters within each group are extensible: they depend on the connected style manifests and the product version.

There is no separate `animation` key in the manifest — animation is connected as a style parameter. For it to work properly:

- the node must have the class `js-animation`
- in `style`, the type `animation` must be specified for this node
- if necessary, you can immediately add an effect class, for example, `fadeIn`

### Key attrs {#attrs-key}

The `attrs` key describes additional block settings whose values are retained in DOM attributes of the elements, `data-view="short"` for example.

For every setting, the name of the DOM attribute is set in the `attribute` field and the type of the field in the editor is set in the `type` field — from a text field and a dropdown list to a palette, an image picker, and a dynamic data source. The `attrs` key is described in four places of the manifest: at the root, inside `style.nodes`, inside `style.block`, and inside `cards`. The place determines which editor form the field appears in.

The full list of types, their fields, and the places of description: [Attributes](./attributes.md#attribute-types).

### Key menu {#menu-key}

The `menu` key is used when a multi-level menu with separate settings for root and child items is needed. For the choice between this key and `cards`, see the [Key cards](#cards-key) section.

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
  - `liClassName` for nested `<li>`
  - `aClassName` for `<a>` links in child items
- `nodes` — editable elements within the menu item, such as a link

Multiple multi-level menus can be described in a single manifest, meaning multiple root selectors in `menu`.

### Key assets {#assets-key}

The `assets` key defines the JS and CSS resources that are connected when adding a block to the page.

- `css` — external CSS files
- `js` — external JS files
- `ext` — Bitrix24 core extensions

If the same file is already included by another block, it will not be added again. There is no need to list the extension dependencies: Bitrix24 connects them itself.

The extensions that blocks use:

#|
|| **Extension** | **What It Connects** | **Where It Is Described** ||
|| `landing_form` | The logic and interfaces of blocks with a CRM form | [Forms in Blocks](./special/crm-forms.md) ||
|| `landing_map` | The map setup interface in a block | [Maps in Blocks](./special/maps.md) ||
|| `landing_carousel` | A slider for cards and images | [Sliders](./interactive/sliders.md) ||
|| `landing_gallery_cards` | Viewing the block images in a separate window | [Galleries](./interactive/gallery.md) ||
|| `landing_countdown` | A countdown timer | [Countdown Timers](./interactive/timer.md) ||
|| `landing_chat` | A chat widget on a site page | No dedicated page ||
|| `landing_google_maps_new` | Nothing of its own; it pulls in `landing_map` | [Maps in Blocks](./special/maps.md) ||
|#

There is also the `landing.widgetvue` extension — it serves the Vue widgets on the Bitrix24 main page. It is not specified in `assets.ext`: the handler of the `widgetvue` subtype assembles `assets`, `nodes`, and `style` for the block itself.

If the script uses libraries loaded by the core, it is better to wrap the initialization in `BX.ready(...)` to ensure the code runs after system connections:

```js
BX.ready(function () {
    // the extensions from assets.ext are already connected here
});
```

### Keys lang_original and lang {#lang-key}

The `lang_original` and `lang` keys define the localization of labels in the block manifest.

- `lang_original` — the original language of the phrases in the manifest
- `lang` — a set of translations by language

Recommendations:

- set `lang_original` according to the actual language of the manifest
- use the same phrase keys in `lang` as in the original manifest

Only the values of `name` keys are translated. For the list of such labels, see [Which Labels Are Translated](./localization.md#translatable-keys).

More details: [Block Localization](./localization.md).

## Permissions and Limitations

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

Limitations:

- the manifest of a standard Bitrix24 block cannot be changed via REST. The `landing.block.*` methods modify a placed block, not the description of its template
- the markup of your own block is passed separately from the manifest — in the `fields.CONTENT` field of the [landing.repo.register](../user-blocks/landing-repo-register.md) method. The selectors from `nodes`, `cards`, and `attrs` have to exist in that markup, otherwise there is nothing for the setting to bind to
- only the core extensions available in the block environment are connected in `assets.ext`

## Continue Your Learning

- [{#T}](./node-types.md)
- [{#T}](./attributes.md)
- [{#T}](./extended-description.md)
- [{#T}](./localization.md)
- [{#T}](./special/index.md)
- [{#T}](./interactive/index.md)
- [{#T}](./methods/landing-block-get-manifest-file.md)
- [{#T}](../user-blocks/landing-repo-register.md)
