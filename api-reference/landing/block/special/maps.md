# Maps in Blocks

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Blocks with a map display an interactive map with markers on the page: an office address, pickup points, or directions. This behavior is enabled through `subtype: map` in the `block` section of the [block manifest](../manifest.md).

The subtype removes the need for manual setup: it describes the map node itself, adds the required attributes, substitutes the initial coordinates, and connects the initialization script. If the map has to be a static image or is embedded by a third-party widget, the subtype is not needed.

The scenario consists of three parts:

- `subtype: map` is specified in the `block` section of the manifest
- the `landing_map` extension is connected in `assets.ext` — it brings the script that builds the map on the page
- the block markup contains the node `.landing-block-node-map`

The node `.landing-block-node-map` has to be present in the markup in advance: the subtype processes only this selector, and without it there is nothing to bind the map to.

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

The subtype handler extends the block manifest:

- describes `.landing-block-node-map` as a node of the `map` type
- adds the `data-map` attribute and, if the provider can be selected, `data-map-provider`
- appends the `map_init` value to `assets.ext`
- determines the current provider from the `data-map-provider` value in the block markup
- disables manifest caching for such a block

When the block is added to a page, the subtype fills in the empty `data-map` attribute: it substitutes the map center, the zoom level `17`, and one marker. The center depends on the Bitrix24 region: different regions use different coordinates. At the same time, the provider is written into the markup according to the rule from the section below.

If `data-map` is already filled in the block markup, the subtype touches neither it nor the provider: the values from the markup are left as is.

If maps are not enabled for the selected provider or the key is not filled in, the subtype adds `requiredUserAction` to the manifest. In the editor, the user will see a required action to navigate to the site settings.

## What Providers Are Used

The provider is stored in the `data-map-provider` attribute of the map node. The allowed values are:

- `google` — Google Maps
- `yandex` — a regional map provider available only in the Russian region of Bitrix24

When a block is added to a page, the subtype selects the provider as follows:

- in the Russian region it writes `yandex`, if that provider is enabled with a key or if no provider is configured at all
- in all other cases it writes `google`

Maps work only with a provider key. The key and the usage flag are set in the site or page settings, and via REST through the additional fields `GMAP_USE` and `GMAP_CODE` for Google Maps and `YMAP_USE` and `YMAP_CODE` for the regional provider. The set of fields is described in the [Additional Site Fields](../../site/additional-fields.md) and [Additional Page Fields](../../page/additional-fields.md) articles.

The provider switch appears in the block settings only where the regional provider is available. In all other regions, the map is always built on Google Maps.

Google Maps additionally supports visual parameters. In the manifest, the subtype declares them not in the root `attrs` key, but in `style.nodes` of the map node — which is why they appear in the design form in the editor. They are still retained in the node attributes, and via REST they are modified by the [landing.block.updateattrs](../methods/landing-block-update-attrs.md) method:

#|
|| **Attribute** | **Values** | **What It Defines** ||
|| `data-map-theme` | Empty string, `SILVER`, `RETRO`, `DARK`, `NIGHT`, `AUBERGINE` | The color theme of the map ||
|| `data-map-roads` | Empty string or `off` | The display of roads ||
|| `data-map-landmarks` | Empty string or `off` | The display of landmarks ||
|#

## Examples of Standard Blocks

Codes for some standard blocks:

- `16.1.google_map`
- `16.2.two_cols_text_map_fix`
- `16.3.two_cols_map_text_fix`
- `16.5.two_cols_map`
- `16.6.two_cols_map_reverse`

## How to Change a Map via REST

For a block already placed on a page, the map settings are retained in the attributes of the `.landing-block-node-map` node: `data-map` holds the center, the zoom level, and the markers in JSON format, and `data-map-provider` holds the provider. The structure of the `data-map` value is described in the [Node Types](../node-types.md) article.

The values of these attributes are modified by the [landing.block.updateattrs](../methods/landing-block-update-attrs.md) method, the current state of the draft is shown by [landing.block.getcontent](../methods/landing-block-get-content.md) with the `editMode = true` parameter, and the changes become visible on the site after the page is published using the [landing.landing.publication](../../page/methods/landing-landing-publication.md) method.

## Permissions and Limitations

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

Limitations:

- standard map blocks connect the `landing_map` extension. The `landing_google_maps_new` extension adds no files of its own and pulls in the same `landing_map`, so it is not used in new blocks
- the initial settings are substituted once, when the block is added to a page. For a block already placed on a page, the values are modified only by [landing.block.updateattrs](../methods/landing-block-update-attrs.md)

## Continue Your Learning

- [{#T}](./index.md)
- [{#T}](./menu.md)
- [{#T}](./navigation.md)
- [{#T}](./search.md)
- [{#T}](./search-forms.md)
- [{#T}](./crm-forms.md)