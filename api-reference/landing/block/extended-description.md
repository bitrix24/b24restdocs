# Extended Description of Cards

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The extended description of cards is a configuration of the `cards` key in the [block manifest](./manifest.md) that allows keeping cards of different kinds in a single list. Regular cards repeat the same markup, while the extended description adds presets to them — card templates with their own markup and initial values.

The extended description of cards is used when a single set of cards requires:

- different sets of fields for cards in the same list, for example just a phone number, or a phone number, an e-mail, and a link
- different layout options for identical objects
- cards from predefined presets

If all the cards in a list are identical, the extended description is not needed — the basic description of the `cards` key is enough.

The configuration is defined by the block author: the manifest and the markup are passed at block registration using the [landing.repo.register](../user-blocks/landing-repo-register.md) method. To see how the `cards` key is filled in a standard block, use the [landing.block.getmanifestfile](./methods/landing-block-get-manifest-file.md) method.

The basic principles of cards and nodes are outlined in the [Block Manifest](./manifest.md) and [Node Types](./node-types.md) articles.

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

#|
|| **Field** | **Value** | **What It Defines** ||
|| `name` | A string | The name of the card group in the interface ||
|| `label` | A node selector or an array of selectors | The rule for forming the card title in the list ||
|| `presets` | An array where the keys are the preset identifiers | A set of card presets. If `presets` is not empty, new cards are added from the presets ||
|| `group_label` | A string | The caption of the card group in the settings form ||
|| `additional` | An object with the `attrs` key | Settings defined separately for each card. The composition is described in the [Attributes](./attributes.md) article ||
|#

### Preset Fields

#|
|| **Field** | **Value** | **What It Defines** ||
|| `name` | A string | The name of the preset in the list ||
|| `html` | HTML markup | The markup of the card for the preset. Only the nodes described in `nodes` and not disabled through `disallow` can be edited ||
|| `values` | An array where the key is a node selector from `nodes` | The initial values of the card nodes when the card is added from the Bitrix24 editor. The format of the value depends on the [node type](./node-types.md) ||
|| `disallow` | An array of selectors | The nodes that cannot be edited in the Bitrix24 editor in this preset ||
|#

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

## How to Add a Card from a Preset via REST

Presets are applied by the [landing.block.updateCards](./methods/landing-block-update-cards.md) method. In the `source` array, pass an element with the `preset` type and the preset code from the manifest:

```json
"source": [
    {
        "type": "card",
        "value": 0
    },
    {
        "type": "preset",
        "value": "telegram"
    }
]
```

The `source` array defines the final composition and order of the block cards, so list the cards you want to keep in it as well. If a non-existent preset is specified in `source`, an empty card appears in its place.

Via REST, only the `html` markup is taken from a preset. The initial values from the `values` key of the manifest are substituted when a card is added in the Bitrix24 editor. Via REST they are set manually: the same `landing.block.updateCards` call has its own `values` key, or the values are recorded afterwards using the [landing.block.updatenodes](./methods/landing-block-update-nodes.md) method.

The `disallow` restriction also applies only in the editor: it hides the fields in the card settings form. The node modification methods do not block such selectors.

The other card methods do not use presets: [landing.block.addcard](./methods/landing-block-add-card.md) adds a card with the HTML passed to it, and [landing.block.clonecard](./methods/landing-block-clone-card.md) copies an existing card by selector.

## Permissions and Limitations

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

Limitations:

- presets work only within the card selector in whose description they are defined
- the [landing.block.updateCards](./methods/landing-block-update-cards.md) method rewrites the content of the card parent in full: any foreign markup inside that container is lost
- an empty `source` does not remove the cards: [landing.block.updateCards](./methods/landing-block-update-cards.md) returns `true` and changes nothing
- [landing.block.updateCards](./methods/landing-block-update-cards.md) does not check the selector passed against the manifest: it works with the block markup. The manifest is needed only for `type: preset` — the preset is looked up in `cards.<selector>.presets`. A selector absent from `cards` is not discarded by the method, and it overwrites the content of the container found by that selector
- the preset markup goes through the sanitizer at block registration. If `manifest.cards[*].presets[*]` contains unsafe content, the [landing.repo.register](../user-blocks/landing-repo-register.md) method returns the `PRESET_CONTENT_IS_BAD` error

## Continue Your Learning

- [{#T}](./manifest.md)
- [{#T}](./node-types.md)
- [{#T}](./attributes.md)
- [{#T}](./localization.md)
- [{#T}](./methods/landing-block-update-cards.md)
- [{#T}](../user-blocks/landing-repo-register.md)