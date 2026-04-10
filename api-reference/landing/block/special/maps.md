# Maps in Blocks

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Blocks with maps use `subtype: map`. This subtype connects map settings, adds the necessary attributes, and prepares the block for use in the editor.

In standard blocks for maps, the following approach is used:

- In the `block` section, `subtype: map` is specified
- In `assets`, the `landing_map` extension is included
- In the block markup, the node `.landing-block-node-map` is used

The node `.landing-block-node-map` must already be present in the block markup. If it is not, the map subtype will not function. For an existing node `.landing-block-node-map`, the handler automatically adds the type `map`, the attribute `data-map`, and, if available, the attribute `data-map-provider`.

## How to Configure a Map Block

The minimal version of the manifest:

```php
'block' => [
    'name' => 'Map',
    'section' => ['contacts'],
    'subtype' => 'map',
],
'assets' => [
    'ext' => ['landing_map'],
],
```

Example markup:

```html
<section class="landing-block g-pt-0 g-pb-0 g-height-70vh">
    <div class="landing-block-node-map h-100"></div>
</section>
```

## What the Map Subtype Does

After adding the block, the system:

- Determines the map provider
- Adds the attribute `data-map-provider` to the node `.landing-block-node-map`
- Creates an initial value for `data-map` with center, zoom, and marker
- Connects the `map_init` extension if it is not already in the manifest

In standard blocks, the manifest usually specifies the `landing_map` extension. The map subtype automatically adds `map_init` if this extension is not present.

By default, the following is recorded in `data-map`:

- Center of the map
- Zoom level `17`
- One initial marker

The starting center of the map depends on the website's zone.

If a map key is not configured for the selected provider, the subtype adds `requiredUserAction`. In the editor, the user will see a required action to navigate to the website settings.

## What Providers Are Used

By default, Google Maps is used. The current provider is stored in the attribute `data-map-provider`.

The starting coordinates of the map depend on the website's zone. For the `ru` zone, a separate set of coordinates is used. For other zones, the default value is applied.

In the block interface, map provider settings are available, and for Google Maps, additional visual parameters are accessible:

- Map theme
- Display of roads
- Display of landmarks

## Important Considerations

- In standard map blocks, the `landing_map` extension is used, not `landing_google_maps_new`
- In standard map manifests, `subtype: map` and `landing_map` are usually specified, while the `map_init` subtype is added automatically
- `type: map` for `.landing-block-node-map` is added by the subtype handler
- If the block does not contain the node `.landing-block-node-map`, the map subtype will not function

## Examples of Standard Blocks

Examples of blocks of this type can be viewed in the repository through the methods [landing.block.getmanifestfile](../methods/landing-block-get-manifest-file.md) and [landing.block.getrepository](../methods/landing-block-get-repository.md).

Codes for some standard blocks:

- `16.1.google_map`
- `16.2.two_cols_text_map_fix`
- `16.3.two_cols_map_text_fix`
- `16.5.two_cols_map`
- `16.6.two_cols_map_reverse`

To ensure the map block functions correctly, add the node `.landing-block-node-map` to the markup in advance, and the `map` subtype will complement the manifest and initial settings.

## Continue Your Learning

- [Special Blocks](./index.md)
- [Menu Blocks](./menu.md)
- [Navigation and Header](./navigation.md)
- [Search Results](./search.md)
- [Search Forms](./search-forms.md)
- [Forms in Blocks](./crm-forms.md)