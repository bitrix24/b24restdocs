# Attributes

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

An attribute is an additional block setting whose value is retained in a DOM attribute of an element, `data-view="short"` for example. The block scripts and CSS read the value from the attribute, while the Bitrix24 editor displays a field of the appropriate kind for it: a list, a checkbox, a palette, or an image picker.

Attributes are described in the `attrs` key of the [block manifest](./manifest.md). The key links a setting with a [node](./node-types.md) or a [card](./extended-description.md) by a CSS selector.

Attributes are used when a setting cannot be expressed through node content:

- block behavior parameters, such as the display mode, the number of cards, or the form address
- parameters for the JS logic of a block: a slider, a gallery, or a countdown timer
- values that the block is styled by conditionally, such as the background color or the badge position

Scope of the topic:

- attributes do not replace nodes. The text, the image, or the link inside an element is described in the `nodes` key, and the design in the `style` key
- this article describes how a setting is declared in the manifest. The attribute values of a block already placed on a page are modified by the [landing.block.updateattrs](./methods/landing-block-update-attrs.md) method

## Where the attrs Key Is Described

The place of the description determines where the field appears in the editor and what the value applies to:

#|
|| **Where `attrs` Is Described** | **Where the Field Is in the Editor** | **What the Value Applies To** ||
|| At the root of the manifest | In the block settings form | To the block elements matching the given selector ||
|| In `style.nodes.<selector>.additional` | In the design form next to the style settings of the element | To the element the styles are defined for ||
|| In `style.block.additional` | In the design form of the whole block | To the block wrapper ||
|| In `cards.<selector>.additional` | In the card settings form | To each card of that selector separately ||
|#

An array of attributes is passed in the value of the selector. If there is only one attribute, it can be described as an object without an array — this is how it is done in the example on the [Search Forms](./special/search-forms.md) page.

Examples for the three most common places:

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

If you need to group some attributes, use the group container `attrs`. In the example below, the key is an empty string: the group of settings relates to the block as a whole. The element the value is retained in is defined by the `selector` field of the specific attribute.

```php
'attrs' => [
    '' => [
        [
            'name' => 'Test group',
            'attrs' => [
                [
                    'type' => 'checkbox',
                    'selector' => '.landing-block-node-catalog',
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
- `items` — the list of options. An option is described by a `name` and `value` pair
- `value` — the default value, such as a string, object, or array
- `selector` — overriding the save selector
- `hidden` — registration without output in the editing interface
- `attrs` — a group of nested attributes

Fields for specific types:

#|
|| **Field** | **For Which Types** | **What It Defines** ||
|| `textOnly` | `text` | Plain text input mode. The field always works in this mode: the `false` value does not enable the visual editor ||
|| `disableLink` | `icon`, `image` | Disabling link editing ||
|| `disableBlocks` | `url` | Disabling block selection in the link selector ||
|| `disableCustomURL` | `url` | Disabling manual input of an arbitrary URL ||
|| `disallowType` | `url` | Disabling the change of the link type in the selector ||
|| `allowedTypes` | `url` | The list of allowed link types, `landing` for site pages for example ||
|| `time` | `date` | Enabling time selection ||
|| `format` | `date` | The format for retaining the date and time ||
|| `dimensions` | `image` | Image size restrictions ||
|| `html` | `filter` | The HTML markup of the filter ||
|| `filterId` | `filter` | The filter identifier ||
|| `selected` inside an option | `multiselect` | A flag of the option selected by default. It is set on an item of the `items` list rather than on the field itself. For the other list types, the default value is set with the `value` field ||
|| `items` inside an option | `multiselect` | A group of nested options ||
|| `placeholder` | `text`, `html`, `date` | A hint for input ||
|| `compact` | `checkbox`, `radio` | Compact display mode for the field ||
|| `property` | `palette`, `position`, `sortable-list`, `checkbox`, `radio`, `multiselect`, `catalog-view`, `filter` | The target CSS property ||
|| `hideSort` | `dynamic_source` | Hiding the sorting of sources ||
|| `sources` | `dynamic_source` | The list of available sources ||
|| `title` | `dynamic_source` | The field title ||
|| `stubText` | `dynamic_source` | The placeholder text ||
|| `useLink` | `dynamic_source` | Enabling link mode ||
|| `linkType` | `dynamic_source` | The link type ||
|#

The necessity of fields depends on `type` and scenario. Generally, `attribute` is required, and for list types, `items` is necessary. The `name` field is recommended for proper display in the interface. The `name` value takes part in manifest translation, see [Which Labels Are Translated](./localization.md#translatable-keys) for details.

Always specify the `type` field. At the root of `attrs`, a description without `type` does not become a field: the system treats it as a group and expects a `name` with a nested `attrs`. Inside groups and in `additional`, a description without `type` is displayed as a dropdown list.

## Attribute Types {#attribute-types}

The attribute type determines what control element will be in the editor and in what format the value will be saved in the element's attribute.

#|
|| **Type** | **Control Element in the Editor** | **Requires the `items` List** ||
|| `text` | A single-line text field | No ||
|| `html` | A multi-line text field | No ||
|| `date` | Date and time selection | No ||
|| `dropdown` | A dropdown list. The `list` value does the same | Yes ||
|| `radio` | Selection of one option from a list | Yes ||
|| `checkbox` | A checkbox or a group of checkboxes | Yes ||
|| `multiselect` | Multiple selection | Yes ||
|| `image` | Image selection | No ||
|| `icon` | Icon selection | No ||
|| `link` | A link with text, an address, and an opening mode | No ||
|| `url` | A simplified link field: the address only, without text and opening mode | No ||
|| `slider` | A scale for selecting a single value | Yes ||
|| `range-slider` | A scale for selecting a range | Yes ||
|| `palette` | Selection from a palette: a set of predefined options in `items` | Yes ||
|| `color` | Selection of an arbitrary color, without a predefined set of options | No ||
|| `sortable-list` | A sortable list of values | Yes ||
|| `position` | Selection of the position or direction of the element | Yes ||
|| `catalog-view` | Settings for displaying catalog data | No ||
|| `filter` | Filter settings | No ||
|| `user-select` | User selection | No ||
|| `dynamic_source` | Selection of a dynamic data source. It works in a block with dynamic cards, see [Search Results](./special/search.md) for details | No ||
|#

The format of the value depends on the type:

- a string — for the text types and for the list types whose `items` values are strings, as well as for `url`, `palette`, `color`, and `position`
- a number — for `date` with `format` set to `ms`, and for `slider` if the `items` values are numeric
- an object — for `link`, `icon`, and `range-slider`
- an array — for `sortable-list` and `multiselect`

Examples for the main types are given below.

## Example with Different Attribute Types

{% cut "Text, Lists, Images, and Links" %}

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

];
```

{% endcut %}

{% cut "Multiple Selection, Scales, and Sorting" %}

```php
$attrs = [
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

];
```

{% endcut %}

{% cut "Link, Date, Palette, and Position" %}

```php
$attrs = [
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

    // palette: palette of values
    '.landing-block-node-palette' => [
        [
            'name' => 'Background Color',
            'type' => 'palette',
            'attribute' => 'data-bg-color',
            'property' => 'background-color',
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

{% endcut %}

## How to Change an Attribute Value via REST

1. Retrieve the block manifest using the [landing.block.getmanifest](./methods/landing-block-get-manifest.md) method and find the required selector in the `attrs` key.
2. Pass the new value using the [landing.block.updateattrs](./methods/landing-block-update-attrs.md) method. The key in the `data` parameter is the element selector, and the value is a set of attributes and their values:

   ```json
   {
       "data": {
           ".landing-block-node-text": {
               "data-view": "full"
           }
       }
   }
   ```

3. Publish the page using the [landing.landing.publication](../page/methods/landing-landing-publication.md) method so that the change appears on the site.

## Permissions and Limitations

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

Limitations:

- the [landing.block.updateattrs](./methods/landing-block-update-attrs.md) method takes the allowed selectors and attributes from the `attrs`, `style.nodes`, `style.block`, and `cards` sections of the manifest. A selector or an attribute absent from them is ignored by the method without an error
- the `attribute` field is required: without it there is nowhere to retain the setting
- the value of an attribute of the `url` type goes through a scheme check when saved. An address with a disallowed scheme is stripped silently, without an error
- the composition of attributes is changed only in your own block: the manifest is passed at registration using the [landing.repo.register](../user-blocks/landing-repo-register.md) method. For standard Bitrix24 blocks, REST changes the values of attributes, not their composition

## Continue Your Learning

- [{#T}](./manifest.md)
- [{#T}](./node-types.md)
- [{#T}](./extended-description.md)
- [{#T}](./localization.md)
- [{#T}](./methods/landing-block-update-attrs.md)
- [{#T}](./methods/landing-block-get-manifest.md)