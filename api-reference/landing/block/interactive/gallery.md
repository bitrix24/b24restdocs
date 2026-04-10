# Galleries

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The gallery uses standard markup for cards and image nodes. In the [block manifest](../manifest.md), include the `landing_gallery_cards` extension.

## How to Set Up the Gallery

The minimal configuration is as follows:

```php
'assets' => [
    'ext' => ['landing_gallery_cards'],
],
```

```html
<div class="js-gallery-cards">
    <img class="landing-block-node-card-img" src="/upload/gallery-1.jpg" data-fancybox="gallery" alt="">
</div>
```

## What the Gallery Extension Does

The `landing_gallery_cards` extension prepares images within `js-gallery-cards` for viewing in gallery mode and creates a clickable `<a>` wrapper around the image. When opened, the image from `src` is used. If the node has a `srcset`, it is also passed to the viewing settings.

## Gallery Markup

The root container of the gallery must have the class `js-gallery-cards`.

Inside the container, image nodes of type `img` are placed. For each image, specify:

- `data-fancybox` — an indicator of participation in the gallery
- `src` — the source of the image that will open upon clicking

Optionally:

- `data-link-classes` — classes that are added to the wrapper link around the image, for example, `d-block g-pos-rel`

Example:

```html
<div class="js-gallery-cards row">
    <div class="landing-block-node-card col-lg-3 col-md-4 col-sm-6 g-mb-30">
        <img
            class="landing-block-node-card-img g-max-width-100x g-max-height-350"
            src="https://cdn.bitrix24.site/bitrix/images/landing/business/270x481/img1.jpg"
            data-fancybox="gallery"
            data-link-classes="d-block g-pos-rel"
            alt="">
    </div>

    <div class="landing-block-node-card col-lg-3 col-md-4 col-sm-6 g-mb-30">
        <img
            class="landing-block-node-card-img g-max-width-100x g-max-height-350"
            src="https://cdn.bitrix24.site/bitrix/images/landing/business/270x481/img2.jpg"
            data-fancybox="gallery"
            data-link-classes="d-block g-pos-rel"
            alt="">
    </div>
</div>
```

## Combining with Carousel

The gallery can be used together with a slider. In this case, it is important to include the extensions in the following order:

1. `landing_carousel`
2. `landing_gallery_cards`

```php
'assets' => [
    'ext' => ['landing_carousel', 'landing_gallery_cards'],
],
```

## Important Considerations

- The image must have a `src` specified; otherwise, the wrapper link will not be created.
- To participate in the gallery, the node must have the `data-fancybox` attribute.
- In the basic scenario, the gallery opens the image from `src`; a separate field for the thumbnail/full version pair is not used.
- `data-link-classes` is applied to the created wrapper link around the image.
- When used together with a slider, first include `landing_carousel`, then `landing_gallery_cards`.

## Continue Your Exploration

- [Sliders](./sliders.md)
- [Countdown Timers](./timer.md)
- [Block Manifest File](../manifest.md)
- [Node Types](../node-types.md)