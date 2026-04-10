# Attributes

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Attributes are additional values that are stored in block elements and used in settings, JS logic, and conditional styling. For example, attributes can be used to store parameters for maps, links, display modes, and other block scenarios.

Attributes are registered under the `attrs` key in the [block manifest](./manifest.md). The `attrs` key links attributes with nodes and cards.

## Where to Describe the attrs Key

The `attrs` key can be defined in several places within the manifest:

1. At the root of the manifest:
```php
'attrs' => [
    '.landing-block-node-text' => [
        [
            'name' => 'Text Setting',
            'type' => 'dropdown',
            'attribute' => 'data-copy',
        ],
    ],
]
```
2. In `style.nodes`, in which case the field is displayed in the design form:
```php
'style' => [
    'nodes' => [
        '.landing-block-node-card-button' => [
            'name' => 'Button',
            'type' => ['border-color', 'button', 'animation'],
            'additional' => [
                'attrs' => [
                    [
                        'type' => 'text',
                        'name' => 'Text field',
                        'attribute' => 'data-test-card-attr',
                    ],
                ],
            ],
        ],
    ],
]
```
3. In `cards`, where the attribute is applied separately to each card:
```php
'cards' => [
    '.landing-block-node-card-button' => [
        'name' => 'Card',
        'additional' => [
            'attrs' => [
                [
                    'type' => 'text',
                    'name' => 'Text field',
                    'attribute' => 'data-test-card-attr',
                ],
            ],
        ],
    ],
]
```

## Grouping Attributes

If you need to group some attributes, use the group container `attrs`:

```php
'attrs' => [
    '' => [
        [
            'name' => 'Test group',
            'attrs' => [
                [
                    'type' => 'checkbox',
                    'selector' => 'bitrix:catalog.section',
                    'name' => '',
                    'items' => [
                        ['name' => 'Product Display', 'value' => '1'],
                        ['name' => 'Product Display 2', 'value' => '2'],
                    ],
                    'attribute' => 'data-checkbox',
                ],
                [
                    'type' => 'checkbox',
                    'name' => '',
                    'items' => [
                        ['name' => 'Product Display 22', 'value' => '1'],
                    ],
                    'compact' => true,
                    'attribute' => 'data-checkbox2',
                ],
            ],
        ],
        [
            'type' => 'checkbox',
            'name' => '',
            'items' => [
                ['name' => 'Product Display 33', 'value' => '1'],
            ],
            'attribute' => 'data-checkbox3',
        ],
    ],
]
```

In the `style.nodes` block, either placement without groups or grouping within a single selector is supported:

```php
'style' => [
    'nodes' => [
        '.landing-block-node-card-button' => [
            'additional' => [
                'attrs' => [
                    [
                        'name' => 'Test group',
                        'attrs' => [
                            [
                                'type' => 'text',
                                'name' => 'Test',
                                'attribute' => 'data-text',
                            ],
                            [
                                'type' => 'text',
                                'name' => 'Test 2',
                                'attribute' => 'data-text2',
                            ],
                        ],
                    ],
                ],
            ],
        ],
    ],
]
```

## Overriding the Selector

If the value should be stored not in the original selector, specify the `selector` for the specific attribute:

```php
[
    'name' => 'Text Field',
    'type' => 'text',
    'attribute' => 'data-text-field',
    'selector' => '.demo-another-selector',
]
```

## Attribute Fields

An attribute has common fields that are used across different types and define the basic settings for the field in the editor. In addition to these, there are type-dependent fields that work only for specific types.

Common fields:

- `name` — the name of the field in the interface
- `attribute` — the name of the DOM attribute where the value is stored
- `type` — the type of the field
- `items` — the list of options
- `value` — the default value, such as a string, object, or array
- `selector` — overriding the save selector
- `hidden` — registration without output in the editing interface
- `attrs` — a group of nested attributes
- `placeholder` — a hint for input
- `compact` — compact display mode for the field

Fields for specific types:

- `textOnly` — simple text input mode without a visual editor, for `text`
- `disableLink` — disable link editing, for `icon` and `image`
- `disableBlocks` — disable block selection in the link selector, for `url`
- `disableCustomURL` — disable manual input of arbitrary URL, for `url`
- `time` — enable time selection, for `date`
- `format` — format for saving date and time, for `date`
- `dimensions` — image size restrictions, for `image`
- `property` — target CSS property, for `palette`, `color`, `position`, `sortable-list`, `catalog-view`, `filter`
- `html` — HTML markup for the filter, for `filter`
- `filterId` — filter identifier, for `filter`
- `hideSort` — hide sorting of sources, for `dynamic_source`
- `sources` — list of available sources, for `dynamic_source`
- `title` — field title, for `dynamic_source`
- `stubText` — placeholder text, for `dynamic_source`
- `useLink` — enable link mode, for `dynamic_source`
- `linkType` — link type, for `dynamic_source`

The necessity of fields depends on `type` and scenario. Generally, `attribute` is required, and for list types, `items` is necessary. The `name` field is recommended for proper display in the interface.

If `type` is not specified, `text` is used by default.

## Attribute Types

The attribute type determines what control element will be in the editor and in what format the value will be saved in the element's attribute.

Attribute types:

- `text` — single-line text field
- `date` — date and time selection
- `html` — multi-line text field
- `dropdown` — dropdown list
- `checkbox` — checkbox or group of checkboxes
- `radio` — selection of one option from a list
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

## Example with Different Attribute Types

```php
$attrs = [
    // text: text field
    '.landing-block-node-text' => [
        [
            'name' => 'Caption',
            'type' => 'text',
            'attribute' => 'data-caption',
            'placeholder' => 'Enter caption',
            'textOnly' => true,
        ],
        // dropdown: list type
        [
            'name' => 'Display Mode',
            'type' => 'dropdown',
            'attribute' => 'data-view',
            'items' => [
                ['name' => 'Short', 'value' => 'short'],
                ['name' => 'Full', 'value' => 'full'],
            ],
            'value' => 'short',
        ],
    ],

    // image: image field with restrictions
    '.landing-block-node-image' => [
        [
            'name' => 'Image',
            'type' => 'image',
            'attribute' => 'data-card-image',
            'dimensions' => [
                'maxWidth' => 1200,
                'maxHeight' => 1200,
            ],
        ],
    ],

    // icon: icon selection
    '.landing-block-node-icon' => [
        [
            'name' => 'Icon',
            'type' => 'icon',
            'attribute' => 'data-card-icon',
            'value' => [
                'classList' => ['fa', 'fa-address-card'],
            ],
        ],
    ],

    // link: link field with text, href, and target
    '.landing-block-node-link' => [
        [
            'name' => 'Link',
            'type' => 'link',
            'attribute' => 'data-card-link',
            'value' => [
                'text' => 'Learn More',
                'href' => '/about',
                'target' => '_self',
            ],
        ],
    ],

    // multiselect: multiple selection, including nested items
    '.landing-block-node-options' => [
        [
            'name' => 'Options',
            'type' => 'multiselect',
            'attribute' => 'data-options',
            'items' => [
                ['name' => 'Option 1', 'value' => '1', 'selected' => true],
                ['name' => 'Option 2', 'value' => '2'],
                [
                    'name' => 'Group',
                    'value' => 'group',
                    'items' => [
                        ['name' => 'Sub-option 1', 'value' => 'group-1', 'selected' => true],
                        ['name' => 'Sub-option 2', 'value' => 'group-2'],
                    ],
                ],
            ],
        ],
    ],

    // slider: selection of a single value from a scale
    '.landing-block-node-slider' => [
        [
            'name' => 'Number of Cards',
            'type' => 'slider',
            'attribute' => 'data-cards-count',
            'items' => [
                ['name' => '1', 'value' => 1],
                ['name' => '2', 'value' => 2],
                ['name' => '3', 'value' => 3],
                ['name' => '4', 'value' => 4],
            ],
            'value' => 2,
        ],
    ],

    // range-slider: selection of a range
    '.landing-block-node-range' => [
        [
            'name' => 'Range of Values',
            'type' => 'range-slider',
            'attribute' => 'data-range',
            'items' => [
                ['name' => '1', 'value' => 1],
                ['name' => '2', 'value' => 2],
                ['name' => '3', 'value' => 3],
                ['name' => '4', 'value' => 4],
                ['name' => '5', 'value' => 5],
            ],
            'value' => [
                'from' => 2,
                'to' => 4,
            ],
        ],
    ],

    // sortable-list: sortable list
    '.landing-block-node-sortable' => [
        [
            'name' => 'Block Order',
            'type' => 'sortable-list',
            'attribute' => 'data-sort-order',
            'items' => [
                ['name' => 'Header', 'value' => 'head'],
                ['name' => 'Properties', 'value' => 'props'],
                ['name' => 'Actions', 'value' => 'action'],
            ],
            'value' => ['head', 'props', 'action'],
        ],
    ],

    // url: link with selection restrictions
    '.landing-block-node-button' => [
        [
            'name' => 'Button Link',
            'type' => 'url',
            'attribute' => 'data-button-url',
            'value' => '#landing166',
            'disableBlocks' => true,
            'disableCustomURL' => false,
        ],
    ],

    // date: date and time with storage format
    '.landing-block-node-date' => [
        [
            'name' => 'Publication Date',
            'type' => 'date',
            'attribute' => 'data-publish-date',
            'time' => true,
            'format' => 'ms',
            'value' => 1621584180000,
        ],
    ],

    // palette: palette reading color from CSS
    '.landing-block-node-palette' => [
        [
            'name' => 'Background Color',
            'type' => 'palette',
            'attribute' => 'data-bg-color',
            'property' => 'background-color',
            // If you need to read the color not from standard styles
            'stylePath' => '/bitrix/templates/my-site/styles.css',
            // If the color is set via a pseudo-element
            'pseudo-element' => '::after',
            // If the color depends on the state of the element
            'pseudo-class' => ':hover',
            'items' => [
                ['name' => 'g-bg-lightblue', 'value' => 'g-bg-lightblue'],
                ['name' => 'g-bg-darkblue', 'value' => 'g-bg-darkblue'],
            ],
        ],
    ],

    // position: selection of position
    '.landing-block-node-badge' => [
        [
            'name' => 'Badge Position',
            'type' => 'position',
            'attribute' => 'data-badge-position',
            'items' => [
                'top-left' => ['content' => '', 'value' => 'top-left'],
                'top-center' => ['content' => '', 'value' => 'top-center'],
                'top-right' => ['content' => '', 'value' => 'top-right'],
            ],
            'value' => 'top-right',
        ],
    ],
];
```