# Maps in Blocks

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet the writing standard

{% endnote %}

{% endif %}

To work with maps (currently only Google Maps are supported) and have full editing functionality, you need to:

1. Place two keys in the **block** section: subtype and subtype_params (see the manifest example for more details).
2. Specify the extension **landing_google_maps_new** (see assets in the manifest example).
3. Indicate the type **map** for the required node (where the map will be).

## Manifest Example

```js
return [
    'block' => [
        'name' => 'Google Map',
        'section' => ['contacts'],
        'subtype' => 'map',
        'subtype_params' =>[
            'required' => 'google'
        ],
    ],
    'cards' => [],
    'nodes' => [
        '.landing-block-node-map' => [
            'name' => 'Map',
            'type' => 'map',
        ]
    ],
    'style' => [
        'block' => [
            'type' => ['block-default-wo-background-vh-animation']
        ],
        'nodes' => [],
    ],
    'assets' => [
        'ext' => ['landing_google_maps_new'],
    ]
];
```

Example block for this manifest:

```html
<section class="landing_block g-pt-0 g-pb-0 g-height-70vh">
     <div class="landing-block-node-map h-100" data-map></div>
</section>
```

You can view examples of blocks of this type in our repository by using the methods [landing.block.getmanifestfile](../methods/landing-block-get-manifest-file.md) and [landing.block.getrepository](../methods/landing-block-get-repository.md). Their codes are:
- 16.3.two_cols_map_text_fix
- 16.4.three_cols_map
- 16.5.two_cols_map
- 16.6.two_cols_map_reverse
- 16.1.google_map
- 16.2.two_cols_text_map_fix
- and many others.