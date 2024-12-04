# Extended Description of Cards

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet the writing standard

{% endnote %}

{% endif %}

To understand the principles of forming and working with cards, it is recommended to first familiarize yourself with the [manifest](./manifest.md).

Extended cards were designed to address the following tasks:

- Different sets of contacts (e-mail and phone, phone only, phone and social media).
- The same set of fields but different graphical representations (social buttons).
- And so on.
All cards are collapsed into compact panels, with quick access to main functions: Sort, Edit, Delete.

Clicking on the panel or the "Edit" button expands the panel into a card editing form.

You can add either an empty card or a card from a preset.

Composite and dynamic card headers are supported. The card title is formed from the data provided by the user. The header can be composed of text, images, icons, and links. Headers will be dynamically updated as the card values are edited.

## Manifest of the Extended Card

```js
'cards' => [
    '.landing-block-card' => [
        'name' => Loc::getMessage('LANDING_BLOCK_4_FEATURES_3_COLS_...'),
        'label' => [
            '.landing-block-node-element-icon',
            '.landing-block-node-element-title'
        ],
        'presets' => [
            'telegram' => [...],
            'instagram' => [...],
            'reddit' => [...],
            'whatsapp' => [...],
            'skype' => [...]
        ]
    ]
]
```

**Key name**

Defines the title of the card group. By default, it is used to form card titles like "Card 1", "Card 2".

**Key label**

Defines the rules for forming the card title. As a value, you can pass a node selector or an array of node selectors from which values should be taken to form the title.

**Key presets**

Defines the set of presets for the current card set. If the property value is not empty, a new card can only be added from a preset. It accepts an array of presets, with the preset identifiers as the keys of the array.

## Preset

```js
'telegram' => [
    'name' => ' Telegram',
    'html' => '<html-code-of-preset>',
    'values' => [
        '.landing-block-node-element-title' => 'Telegram',
        '.landing-block-node-element-text' => 'Any text ...',
        '.landing-block-node-element-icon' => [
            'type' => 'icon',
            'classList' => [
                'landing-block-node-element-icon',
                'fa',
                'fa-telegram'
            ]
        ]
    ],
    'disallow' => [
        '.landing-block-node-element-icon'
    ]
]
```

**Key name**

Defines the title of the preset. Supports HTML. Displayed in the dropdown list of presets.

**Key html**

The layout of the card. It may differ from the layout of other cards. When adding layout to a preset, it should be noted that only those nodes defined in nodes and not overridden in disallow will be editable.

**Key values**

Defines the values of nodes and fields of the card with which they will be initialized. The key is the node selector from nodes, and the value is the node value according to the node type.

**Key disallow**

Defines which nodes the user will not be able to edit. An array of selectors is expected as the value.

## Layout of the Preset

The layout of one preset is a repeatable block of code.

Example:

```html
<li class="landing-block-node-list-item col g-min-width-65 list-inline-item g-mr-0"
	data-card-preset="telegram">
	<a class="landing-block-node-list-item-link d-block g-py-15 g-px-30 g-bg-telegram--hover g-bg-telegram g-color-white text-center" href="#">
		<i class="landing-block-node-list-item-icon fa fa-telegram"></i>
	</a>
```

Ideologically, unlike a regular card, such repeatable contents can change. For example, in one `<li>` in the example above, there may be no link, or an image may be present instead.

{% note info %}

Please note that the content of the cards may differ, but it is better to keep the outer block consistent.
Also, make sure to specify `data-card-preset="<preset code>"` in both the preset and the layout (see the example above).

{% endnote %}