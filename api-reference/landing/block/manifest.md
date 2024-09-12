# Manifest File

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet the writing standard

{% endnote %}

{% endif %}

The manifest file accompanies each [block](./index.md) and describes the editable parts of the block, as well as containing its name, description, and JS/CSS files.

## Examples

{% note warning %}

The output of the block must contain at least one tag, even if it is technical information. 
For example: `<div>This block works only after publication</div>`.

{% endnote %}

Example of a block:

```html
<section class="landing-block container-fluid px-0 g-theme-business-bg-blue-dark-v1">
    <div class="row no-gutters align-items-start">

        <div class="landing-block-card js-animation fadeIn col-md-6 col-lg-6 g-flex-centered">
            <div class="landing-block-node-card-container text-center g-pa-30 w-100">
                <div class="landing-block-node-card-header text-uppercase u-heading-v2-4--bottom g-brd-primary g-mb-40">
                    <h2 class="landing-block-node-title h1 u-heading-v2__title g-line-height-1_3 g-font-weight-600 g-font-size-40 g-color-white g-mb-minus-10">Help
make <br> money</h2>
                </div>

                <div class="landing-block-node-text g-color-gray-light-v2">
                    <p>Sed feugiat porttitor nunc, non dignissim ipsum vestibulum in. Donec in blandit dolor. Vivamus a fringilla lorem, vel faucibus ante. Nunc
ullamcorper, justo a iaculis elementum, enim orciviverra eros, fringilla porttitor lorem eros vel odio. Praesent egestas ac arcu ac convallis. Donec ut diam
risus purus.</p>
                </div>
            </div>
        </div>

        <div class="landing-block-card js-animation fadeIn col-md-6 col-lg-6 g-flex-centered">
            <div class="landing-block-node-card-container text-center g-pa-30 w-100">
                <div class="landing-block-node-card-header text-uppercase u-heading-v2-4--bottom g-brd-primary g-mb-40">
                    <h2 class="landing-block-node-title h1 u-heading-v2__title g-line-height-1_3 g-font-weight-600 g-font-size-40 g-color-white g-mb-minus-10">Help
make <br> money</h2>
                </div>

                <div class="landing-block-node-text g-color-gray-light-v2">
                    <p>Sed feugiat porttitor nunc, non dignissim ipsum vestibulum in. Donec in blandit dolor. Vivamus a fringilla lorem, vel faucibus ante. Nunc
ullamcorper, justo a iaculis elementum, enim orciviverra eros, fringilla porttitor lorem eros vel odio. Praesent egestas ac arcu ac convallis. Donec ut diam
risus purus.</p>
                </div>
            </div>
        </div>

    </div>
</section>
```
{% note info %}

Maintaining unique markup in the manifest is important only within a single block. Between blocks, identical selectors can have completely different meanings.

{% endnote %}

Here is how the manifest file for the block above might look:

```php
<?php
if (!defined('B_PROLOG_INCLUDED') || B_PROLOG_INCLUDED !== true)
{
    die();
}

use \Bitrix\Main\Localization\Loc;

return array(
    'block' => array(
        'name' => "Text in Two Columns",
        'section' => array('columns', 'text'),
    ),
    'cards' => array(
        '.landing-block-card' => array(
            'name' => "Column",
            'label' => array(
                '.landing-block-node-subtitle',
                '.landing-block-node-title',
            ),
        ),
    ),
    'nodes' => array(
        '.landing-block-node-title' => array(
            'name' => "Title",
            'type' => 'text',
        ),
        '.landing-block-node-text' => array(
            'name' => "Text",
            'type' => 'text',
        ),
    ),
    'style' => array(
        'block' => array(
            'block-default',
        ),
        'nodes' => array(
            '.landing-block-card' => array(
                'name' => "Column",
                'type' => array('columns', 'animation'),
            ),
            '.landing-block-node-title' => array(
                'name' => "Title",
                'type' => 'typo',
            ),
            '.landing-block-node-text' => array(
                'name' => "Text",
                'type' => 'typo',
            ),
            '.landing-block-node-card-header' => array(
                'name' => "Title",
                'type' => 'border-color',
            ),
        ),
    ),
    'attrs' => array(
        '.landing-block-node-text' => array(
            'name' => 'Text Settings',
            'type' => 'dropdown',
            'attribute' => 'data-copy',
            'items' => array(
                'val1' => 'Value 1',
                'val2' => 'Value 2',
            ),
        ),
    ),
    
    'assets' => array(
        'css' => array(
            'https://site.com/aaa.css',
        ),
        'js' => array(
            'https://site.com/aaa.js',
        ),
        'ext' => array(
            'landing_form',
        ),
    ),
);
```

## Manifest Fields of the Block

### Key block

The **block** key contains the name and category of the block (or an array of categories). There are a number of categories in the system, here they are:

```js
array(
    'cover' => Cover,
    'about' => About the Project,
    'title' => Title,
    'text' => Text Block,
    'image' => Image,
    'gallery' => Gallery,
    'phrase' => Quote,
    'benefits' => Benefits,
    'columns' => Columns,
    'separator' => Separator,
    'menu' => Menu,
    'footer' => Website Footer,
    'pages' => List of Pages,
    'tiles' => Tile and Link,
    'forms' => CRM Form,
    'team' => Team,
    'feedback' => Reviews,
    'schedule' => Schedule,
    'steps' => Steps,
    'contacts' => Contacts,
    'social' => Social Networks,
    'tariffs' => Tariffs,
    'partners' => Partners,
    'other' => Other,
    'popular' => Popular,
    'sidebar' => Sidebar,
    'video' => Video,
    'text_image' => Text with Images,
    'countdowns' => Timers for Promotions,
    'separator' => Transitions and Separators,
    'news' => News Feed,
);
```

If the desired category is not in the list, simply write its text in the manifest, and the category will be added.

In addition, this key can contain the following settings:

- **dynamic** => false tells the block that it CANNOT be used as dynamic content. (A very rare situation.)
- **subtype** – type of special block, allows a single value or multiple values in the form of an array. (For on-premise.)
- **subtype_params** – parameters of the special block. (For on-premise.)
- **type** - can contain the type of site on which the block can operate (by default, the block is shown everywhere). Currently supported:
  - page: regular websites
  - store: stores
  - knowledge: knowledge bases
  - group: knowledge bases of social network groups

### Key menu

The menu on the site can very well be a regular HTML block, and a whole section in the block catalog is allocated for such blocks. The links of the menu items are simply edited like regular cards. But how to build a multi-level menu? 

For this, a separate entry in the manifest with the key menu is intended. Here’s how such an entry looks:

```php
'menu' => [
    '.landing-block-node-menu' => [
        'item' => '.landing-block-node-menu-item',
        'name' => Loc::getMessage('LANDING_BLOCK_MENU_22-NAVBAR'),
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
                'name' => Loc::getMessage('LANDING_BLOCK_MENU_22-LINK'),
                'type' => 'link',
            ],
        ],
    ],
]
```

This entry describes the following HTML:

```html
<ul class="landing-block-node-menu navbar-nav">
    <li class="landing-block-node-menu-item nav-item">
        <a href="#" target="_self" class="landing-block-node-link nav-link">Bitrix24 Knowledge Base</a>
    </li>
    <li class="landing-block-node-menu-item">
        <a href="#" target="_self" class="landing-block-node-link nav-link">Tasks and Projects</a>
        <ul class="landing-block-node-menu">
            <li class="landing-block-node-menu-item nav-item">
            <a href="#" target="_self" class="landing-block-node-link nav-link">Article example</a>
            </li>
        </ul>
    </li>
</ul>
```

Let’s consider the keys of the menu block:

- **Root of the array** (.landing-block-node-menu in this case) indicates the selector of the `<ul>` tag that should be defined as a menu. As you have already understood, there can be several multi-level menus within a single manifest.
- **item** describes each `<li>` inside the `<ul>` list. That is, those `<li>` tags that will be defined as menu items.
- **name** describes the name of the menu.
- **root** describes the root `<ul>`. And **children** all root `<ul>`. `ulClassName` describes the classes of the `<ul>` tag, `liClassName` describes the classes of the `<li>` tag, and `aClassName` describes the classes of the `<a>` tag. In this case, the parent and child elements have the same structure. No additional tags are currently anticipated in the menu structure.
- **nodes** describes the structure inside the `<li>`. Currently, this is just a link. A careful reader may guess that this item can describe any complex structure of each item. Correct, but this is not fully supported yet.

### Key assets

The **assets** key contains the JS and CSS that need to be included when adding the block to the page. If several blocks use the same JS/CSS file, it’s not a problem; each file will only be included once.

The **ext** key refers to JS libraries of the Bitrix core. Currently, only libraries mentioned in special blocks and in interactive blocks are allowed to be included.

If you are using any third-party libraries in your code that are already included in the main core (for example, jQuery), it is recommended to wrap the output commands in your script in a system method. This way, your code will successfully initialize after all system inclusions:

```js
BX.ready(function()
{
    console.log($(window));
});
```

### Key nodes

The **nodes** key contains blocks whose content is allowed to be edited. Here and further, the ideology of **CSS selectors** is applied to indicate the final nodes. You should understand this to get a good grasp of the further API of blocks.

It is recommended to choose a descriptive class name as a selector. To distinguish regular classes from structural class selectors, it is advisable to give the name a descriptive prefix, for example, `"landing-block-node-"`. The same selector can be used in different blocks. The node selector **does not match** the [card selectors](#cards) of this block.

In the nodes key, selectors whose content can be edited are listed as keys, along with the name of the node and its type. You can read more about [node types](#).

Depending on the type of node and its indication in this block, it will become available for editing and will appear in the block content editing form.

In addition to the type, names, and specific keys of a particular type, there are also common parameters for nodes of any type:

- **allowInlineEdit** – if the value of the key is set to `false`, then the data of the nodes will be prohibited from inline editing, but will still be available for editing through the block editing form.
- **useInDesigner** - if set to `false`, the element will be ignored in the block designer.
- **group** – grouping of nodes; if the same value of this key is specified for several nodes (within one block), then clicking on any of the nodes will open the editing interface for the entire group of nodes.

### Key style

The **style** key is very similar to the nodes key, except that this key specifies the allowed design: which nodes are allowed to change their appearance, and what type they are.

Style types can be the following (including their combination in the form of an array):

- **box** – block elements
- **button** – links in the form of buttons
- **typo** – all typography
- **typo-simple** – simplified typography
- **typo-link** – typography of links
- **paddings** – all paddings
- **navbar** – navigation blocks
- **navbar-full** – full-width navigation blocks
- and some others...

These types determine what stylistic changes are available for editing in the user interface.

{% cut "Full list of allowed types and what CSS styles it combines" %}

```js
array (
    'block-default' => array (
        0 => 'display',
        1 => 'padding-top',
        2 => 'padding-bottom',
        3 => 'padding-left',
        4 => 'padding-right',
        5 => 'margin-top',
        6 => 'background-color',
        7 => 'background-gradient',
    ),
    'paddings' => array (
        0 => 'padding-top',
        1 => 'padding-bottom',
        2 => 'padding-left',
        3 => 'padding-right',
    ),
    'block-default-background-overlay' => array (
        0 => 'display',
        1 => 'background-attachment',
        2 => 'padding-top',
        3 => 'padding-bottom',
        4 => 'padding-left',
        5 => 'padding-right',
        6 => 'margin-top',
        7 => 'background-overlay',
    ),
    'block-default-background-overlay-paddings-x' => array (
        0 => 'display',
        1 => 'background-attachment',
        2 => 'padding-left',
        3 => 'padding-right',
        4 => 'margin-top',
        5 => 'background-overlay',
    ),
    'block-default-background-overlay-height-vh' => array (
        0 => 'display',
        1 => 'background-attachment',
        2 => 'height-vh',
        3 => 'padding-top',
        4 => 'padding-bottom',
        5 => 'padding-left',
        6 => 'padding-right',
        7 => 'margin-top',
        8 => 'background-overlay',
    ),
    'block-default-wo-background' => array (
        0 => 'display',
        1 => 'padding-top',
        2 => 'padding-bottom',
        3 => 'padding-left',
        4 => 'padding-right',
        5 => 'margin-top',
    ),
    'block-default-wo-background-height-vh' => array (
        0 => 'display',
        1 => 'height-vh',
        2 => 'padding-top',
        3 => 'padding-bottom',
        4 => 'padding-left',
        5 => 'padding-right',
        6 => 'margin-top',
    ),
    'block-default-wo-paddings' => array (
        0 => 'display',
        1 => 'background-color',
        2 => 'background-gradient',
    ),
    'block-default-wo-background-vh-animation' => array (
        0 => 'display',
        1 => 'padding-top',
        2 => 'padding-bottom',
        3 => 'padding-left',
        4 => 'padding-right',
        5 => 'margin-top',
        6 => 'height-vh',
        7 => 'animation',
    ),
    'typo' => array (
        0 => 'text-align',
        1 => 'color',
        2 => 'font-size',
        3 => 'font-family',
        4 => 'font-weight',
        5 => 'text-decoration',
        6 => 'text-transform',
        7 => 'line-height',
        8 => 'letter-spacing',
        9 => 'text-shadow',
    ),
    'typo-simple' => array (
        0 => 'font-size',
        1 => 'font-family',
        2 => 'font-weight',
        3 => 'text-decoration',
        4 => 'text-transform',
        5 => 'line-height',
        6 => 'letter-spacing',
    ),
    'typo-link' => array (
        0 => 'color',
        1 => 'color-hover',
        2 => 'font-size',
        3 => 'font-family',
        4 => 'font-weight',
        5 => 'text-decoration',
        6 => 'text-transform',
        7 => 'letter-spacing',
        8 => 'text-shadow',
    ),
    'box' => array (
        0 => 'background-color',
        1 => 'background-gradient',
        2 => 'box-shadow',
        3 => 'opacity',
        4 => 'padding-top',
        5 => 'padding-bottom',
    ),
    'bg' => array (
        0 => 'background-color',
        1 => 'background-gradient',
    ),
    'button' => array (
        0 => 'button-color',
        1 => 'button-outline-color',
        2 => 'border-width',
        3 => 'border-radius',
        4 => 'color',
        5 => 'font-family',
        6 => 'font-weight',
        7 => 'text-decoration',
        8 => 'text-transform',
    ),
    'navbar' => array (
        0 => 'navbar-align',
        1 => 'navbar-color',
        2 => 'navbar-color-hover',
    ),
    'navbar-full' => array (
        0 => 'navbar-align',
        1 => 'navbar-color',
        2 => 'navbar-color-hover',
        3 => 'navbar-color-fix-moment',
        4 => 'navbar-color-fix-moment-hover',
    ),
)
```

{% endcut %}

### Key cards

Contains the so-called cards of the block. This is repeatable content. For example, a list of services, photos of employees, and so on. By marking the block this way, the interface will have functionality for cloning, deleting, and modifying cards.

In its simplest form, it looks as indicated in the manifest. If you want, you can also explore advanced card management.

It is recommended to choose a descriptive class name as a selector. To distinguish regular classes from structural class selectors, it is advisable to give the name a descriptive prefix. For example, `landing-block-card-`. The same selector can be used in different blocks. The card selector **does not match** the [node selectors](#nodes) of this block.

{% note info %}

Cards of one selector should not be with cards of another selector in one common parent. For cases with a common parent, use advanced cards.

{% endnote %}

### Key attrs

See the description of attributes on the [separate page](./attributes.md).

## Ideology of Changing Block Styles

When changing the appearance of blocks, the **style** attribute of nodes is not changed. Only classes are changed. For example, if you want to change the font-size from 12 to 16, the system will change not 'font-size', but the conditional class g-fontsize12 to g-fontsize16.

## For Animation to Work

In the standard mechanism for animation to work, it is necessary:

- for the node to have the class js-animation;
- for the manifest of this node to have a setting in the style section - 'type' => 'animation';
- for the animation to work immediately by default, it is also necessary to add one of the animation classes (in our block example - fadeIn), but this is not mandatory; the setting will work as is.

