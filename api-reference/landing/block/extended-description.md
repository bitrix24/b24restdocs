# Extended Description of Cards

The extended description of cards is an advanced configuration of the `cards` key in the manifest, which allows for multiple card variations within a single list.

The basic principles of cards and nodes are outlined in the [block manifest](./manifest.md).

The extended description of cards is used when a single set of cards requires:

- different sets of fields for cards in the same list, for example, just a phone number or phone number + e-mail + link
- different layout options for identical objects
- cards from predefined presets

## Example of an Extended Card Description

```php
'cards' => [
    '.landing-block-card' => [
        'name' => 'Contacts',
        'label' => [
            '.landing-block-node-element-icon',
            '.landing-block-node-element-title',
        ],
        'presets' => [
            'telegram' => [
                'name' => 'Telegram',
                'html' => '<html-preset-code>',
                'values' => [
                    '.landing-block-node-element-title' => 'Telegram',
                    '.landing-block-node-element-text' => 'Any text ...',
                    '.landing-block-node-element-icon' => [
                        'type' => 'icon',
                        'classList' => [
                            'landing-block-node-element-icon',
                            'fa',
                            'fa-telegram',
                        ],
                    ],
                ],
                'disallow' => [
                    '.landing-block-node-element-icon',
                ],
            ],
        ],
    ],
],
```

## Fields of the Extended Card Description

- `name` — the name of the card group in the interface
- `label` — the rule for forming the card title. You can specify either a single node selector or an array of selectors
- `presets` — a set of card presets. If `presets` is not empty, new cards are added from the presets. The value is an array where the keys are the preset identifiers

### Preset Fields

- `name` — the name of the preset in the list
- `html` — the markup for the card in the preset. Only those nodes defined in `nodes` and not disabled via `disallow` can be edited
- `values` — initial values for the nodes and fields of the card. The key is the node selector from `nodes`, and the value is the data in the format corresponding to the node type
- `disallow` — a list of node selectors that cannot be edited

## Preset Markup

To link a card to a preset in the markup, specify the `data-card-preset` attribute with the preset code. The value of `data-card-preset` must match the key of the preset in `presets`.

The internal structure of the card may differ across different presets. For example, one variant may use a link inside `<li>`, while another may use an image instead of a link. However, it is recommended to keep the external container of the card uniform.

Example:

```html
<li class="landing-block-node-list-item col g-min-width-65 list-inline-item g-mr-0"
    data-card-preset="telegram">
    <a class="landing-block-node-list-item-link d-block g-py-15 g-px-30 g-bg-telegram--hover g-bg-telegram g-color-white text-center" href="#">
        <i class="landing-block-node-list-item-icon fa fa-telegram"></i>
    </a>
</li>
```