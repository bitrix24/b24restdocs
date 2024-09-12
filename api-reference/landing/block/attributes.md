# Attributes

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet the writing standard

{% endnote %}

{% endif %}

Using the **attrs** key in the [manifest](./manifest.md) of the block specifies a list of attributes for storing data associated with specific nodes. This is used everywhere – from default field values, counters to map settings, videos, and much more. Typically, a specific script accompanies the set of attributes that can work with all of this. Alternatively, attributes can participate in styling blocks by indicating in CSS that a card with a specific attribute has a different color (for example).

Each attribute is described by:

- name,
- code,
- type,
- **items** key (in the case of a list type).

## Placement Locations

The **attrs** key in the manifest can be placed in the following locations:

1. Directly at the root, as shown in the manifest example.
```js
'attrs':
{
    '.landing-block-node-text':
    {
        'name': 'Text settings',
        'type': 'dropdown',
        'attribute': 'data-copy'
    }
},
```
2. In the style key, in which case the attribute is displayed in the form of design settings.
```js
'style':
{
    '.landing-block-node-card-button':
    {
        'name': 'Button',
        'type': ['border-color', 'button', 'animation'],
        'additional': {
            'attrs': [
                [
                    'type': 'text',
                    'name': 'Text field',
                    'attribute': 'data-test-card-attr'
                ]
            ]
        }
    },
}
```
3. In the card description. In this case, the attribute is applied directly to each card separately.
```js
'cards':
{
    '.landing-block-node-card-button':
    {
        'name': 'Card',
        'additional': {
            'attrs': [
                [
                    'type': 'text',
                    'name': 'Text field',
                    'attribute': 'data-test-card-attr'
                ]
            ]
        }
    },
}
```

## Grouping Attributes

If you need to group some attributes, you can do it as follows:

```js
// root placement
'attrs' => array(
    '' => array(
        array(
            'name' => 'Test group',
            'attrs' => array(
                array(
                    "type" => "checkbox",
                    // Override selector (if needed)
                    "selector" => "bitrix:catalog.section",
                    "name" => "",
                    "items" => array(
                        array("name" => "Display products", "value" => "1"),
                        array("name" => "Display products 2", "value" => "2"),
                        array("name" => "Display products 3", "value" => "3"),
                    ),
                    "attribute" => "data-checkbox"
                ),
                array(
                    "type" => "checkbox",
                    "name" => "",
                    "items" => array(
                        array("name" => "Display products 22", "value" => "1")
                    ),
                    "compact" => true,
                    "attribute" => "data-checkbox2"
                )
            )
        ),
        array(
            "type" => "checkbox",
            "name" => "",
            "items" => array(
                array("name" => "Display products 33", "value" => "1")
            ),
            "attribute" => "data-checkbox3"
        )
    )
)
// style block (note that in the case of style, only either no groups or grouping within a single selector is supported)
'additional' => array(
    array(
        'name' => 'Test group',
        'attrs' => array(
            array(
                "type" => "text",
                "name" => "Test",
                "attribute" => "data-text"
            ),
            array(
                "type" => "text",
                "name" => "Test 2",
                "attribute" => "data-text2"
            )
        )
    )
)
```

## Different Selectors

If you want the attribute values to be saved in a different selector, simply specify a different selector for that specific attribute. (This can be useful to avoid adding unnecessary nodes for visual changes):

```js
array(
    'name' => 'Text field',
    'type' => 'text',
    'attribute' => 'data-text-field',
    'selector' => '.demo-another-selector'
)
```

## Attribute Types

Attributes are a conditional storage for hidden values. For example, the starting coordinates of a map. Naturally, attributes make sense to introduce only in conjunction with some JS code that can use these attributes. Attributes need to be [registered in the manifest](./manifest.md) under the **attrs** key.

Currently, the following attribute types are supported:

- **text** - a regular text string.
- **html** - a multiline text field.
- **images** - an image with standard controls - selection from the computer or search in libraries.
- **icon** - an icon.
- **dropdown** - a dropdown list.
- **checkbox** - a group of checkboxes. If you want to display a single checkbox, simply specify one value in items.
- **multiselect** - a multiple selection list.
- **link** - a link with standard controls.
- **url** - a simplified version of a link: selection of a page/block or arbitrary URL.
- **slider** / **range-slider** - slider options for an array of values.
- **palette** - a color palette.
- **sortable-list** - a sortable list of values. Sorting occurs by dragging elements with the mouse.
- **position** - a set of arrows to indicate the position of an element in a block.
- **date** - date and time selection.

Specific examples with these types can be found below. There you will also find additional options for variability.

**Additionally**

In addition to the specific properties of each type (see the example below), each type can have additional properties:

- **hidden** - the attribute is registered but not displayed for editing in the block card, convenient for registering blocks when the sanitizer does not allow unregistered attributes.

### Example

```php
<?php
$attrs = array(
    ".landing-node" => array(
        array(
            "type" => "text",
            "name" => "Test attr field",
            "placeholder" => "Type your text",
            "value" => "default_value",
            "attribute" => "data-test-text",
            "textOnly" => false //if true, the editor will not be connected during editing
        ),
    ),
    array(
        "type" => "image",
        "name" => "Test attr image field",
        "value" => array(
            "src" => "http://bitrix24.com/bitrix/images/landing/app-store-badge.svg",
            "alt" => "test alt"
        ),
        "attribute" => "data-test-image"
    ),
    array(
        "type" => "icon",
        "name" => "Test attr icon field",
        "value" => array(
            "classList" => array("fa", "fa-address-card")
        ),
        "attribute" => "data-test-icon"
    ),
    array(
        "type" => "dropdown",
        "name" => "Test attr dropdown field",
        "items" => array(
            array("name" => "#1", "value" => 1),
            array("name" => "#2", "value" => 2),
            array("name" => "#3", "value" => 3),
            array("name" => "#4", "value" => 4)
        ),
        "value" => 3,
        "attribute" => "data-test-dropdown"
    ),
    array(
        'name' => 'Checkbox field',
        'type' => 'checkbox',
        'attribute' => 'data-test-checkbox',
        'items' => array(
            array('name' => 'Allow specifying product quantity', 'value' => '1', 'checked' => true),
            array('name' => 'Allow notifications for out-of-stock products', 'value' => '2', 'checked' => true),
            array('name' => 'Show discount percentage', 'value' => '3', 'checked' => true),
            array('name' => 'Show old price', 'value' => '4', 'checked' => true),
            array('name' => 'Allow product comparison', 'value' => '5', 'checked' => true)
        )
    ),
    array(
        'name' => 'Multi select field',
        'type' => 'multiselect',
        'attribute' => 'data-test-multiselect',
        'items' => array(
            array('name' => 'Allow specifying product quantity', 'value' => '1', 'selected' => true),
            array('name' => 'Allow notifications for out-of-stock products', 'value' => '2', 'selected' => true),
            array('name' => 'Show discount percentage', 'value' => '3'),
            array('name' => 'Show old price', 'value' => '4', 'items' => array(
                array('name' => 'Allow product comparison', 'value' => '41', 'selected' => true),
                array('name' => 'Allow specifying product quantity', 'value' => '42', 'selected' => true),
                array('name' => 'Allow notifications for out-of-stock products', 'value' => '43', 'selected' => true),
                array('name' => 'Show discount percentage', 'value' => '44', 'selected' => true)
            )),
            array('name' => 'Allow product comparison', 'value' => '5'),
            array('name' => 'Allow specifying product quantity', 'value' => '6'),
            array('name' => 'Allow notifications for out-of-stock products', 'value' => '7', 'selected' => true)
        )
    ),
    array(
        "type" => "link",
        "name" => "Test attr link field",
        "value" => array(
            "text" => "Link anchor",
            "href" => "/test",
            "target" => "_popup"
        ),
        "attribute" => "data-test-link"
    ),
    array(
        "type" => "slider",
        "name" => "Test attr slider field",
        "items" => array(
            array("name" => "1", "value" => 1),
            array("name" => "2", "value" => 2),
            array("name" => "3", "value" => 3),
            array("name" => "4", "value" => 4),
            array("name" => "5", "value" => 5)
        ),
        "value" => 2,
        "attribute" => "data-test-slider"
    ),
    array(
        "type" => "range-slider",
        "name" => "Test attr range slider field",
        "items" => array(
            array("name" => "1", "value" => 1),
            array("name" => "2", "value" => 2),
            array("name" => "3", "value" => 3),
            array("name" => "4", "value" => 4),
            array("name" => "5", "value" => 5)
        ),
        "value" => array(
            "from" => 3,
            "to" => 5
        ),
        "attribute" => "data-test-range-slider"
    ),
    array(
        "type" => "palette",
        "name" => "Test attr palette field",
        "items" => array(
            array('name' => 'g-bg-lightblue', 'value' => 'g-bg-lightblue'),
            array('name' => 'g-bg-lightblue-opacity-0_1', 'value' => 'g-bg-lightblue-opacity-0_1'),
            array('name' => 'g-bg-lightblue-v1', 'value' => 'g-bg-lightblue-v1'),
            array('name' => 'g-bg-lightblue-v1-opacity-0_1', 'value' => 'g-bg-lightblue-v1-opacity-0_1'),
            array('name' => 'g-bg-darkblue', 'value' => 'g-bg-darkblue'),
            array('name' => 'g-bg-darkblue-opacity-0_1', 'value' => 'g-bg-darkblue-opacity-0_1'),
            array('name' => 'g-bg-indigo', 'value' => 'g-bg-indigo'),
            array('name' => 'g-bg-indigo-opacity-0_1', 'value' => 'g-bg-indigo-opacity-0_1'),
            array('name' => 'g-bg-red', 'value' => 'g-bg-red'),
            array('name' => 'g-bg-red-opacity-0_1', 'value' => 'g-bg-red-opacity-0_1'),
            array('name' => 'g-bg-red-opacity-0_2', 'value' => 'g-bg-red-opacity-0_2'),
            array('name' => 'g-bg-red-opacity-0_5', 'value' => 'g-bg-red-opacity-0_5'),
            array('name' => 'g-bg-red-opacity-0_8', 'value' => 'g-bg-red-opacity-0_8'),
            array('name' => 'g-bg-lightred', 'value' => 'g-bg-lightred'),
            array('name' => 'g-bg-lightred-opacity-0_1', 'value' => 'g-bg-lightred-opacity-0_1'),
            array('name' => 'g-bg-darkred', 'value' => 'g-bg-darkred'),
            array('name' => 'g-bg-darkred-opacity-0_1', 'value' => 'g-bg-darkred-opacity-0_1'),
            array('name' => 'g-bg-purple', 'value' => 'g-bg-purple')
        ),
        "property" => "background-color",
        "attribute" => "data-test-palette",

        // Set if you need to get color by className from css
        // "stylePath" => "/path/to/stylesheet.css",

        // Set if you need to get color from styles for pseudo-element (::before, ::after)
        // "pseudo-element" => "::after",

        // Set if you need to get color from styles for pseudo-class (:hover, :active, ...)
        // "pseudo-class" => ":hover"
    ),
    array(
        "type" => "sortable-list",
        "name" => "Product blocks",
        "items" => array(
            array("name" => 'head', "value" => "1"),
            array("name" => "props", "value" => "2"),
            array("name" => "tp", "value" => "3"),
            array("name" => "qant", "value" => "4"),
            array("name" => "quant2", "value" => "5"),
            array("name" => "action", "value" => "6"),
            array("name" => "comp", "value" => "7")
        ),
        "value" => array("1", "2", "3", "4", "5", "6", "7"),
        "attribute" => "data-catalog-prop-sort"
    ),
    array(
        "type" => "position",
        "name" => "position",
        "items" => array(
            "top-left" => array("content" => "", "value" => "1"),
            "top-center" => array("content" => "", "value" => "2"),
            "top-right" => array("content" => "", "value" => "3"),
            "middle-left" => array("content" => "", "value" => "4"),
            "middle-center" => array("content" => "", "value" => "5"),
            "middle-right" => array("content" => "", "value" => "6"),
            "bottom-left" => array("content" => "", "value" => "7"),
            "bottom-right" => array("content" => "", "value" => "8")
        ),
        "value" => "top-right",
        "attribute" => "data-catalog-prop-position"
    ),
    array(
        'name' => 'URL field',
        'type' => 'url',
        'value' => '#landing166',
        'attribute' => 'data-test-url',
        'disableBlocks' => true, // Disables block selection
        'disableCustomURL' => false // Disables the ability to enter the URL manually
    ),
    array(
        'name' => 'Datetime',
        'type' => 'date',
        'time' => true,//allow selection of exact time
        'format' => 'ms',//'ms' (milliseconds) / 's' (seconds)
        'value' => 1621584180000
    )
);
```

{% include [Note on examples](../../../_includes/examples.md) %}