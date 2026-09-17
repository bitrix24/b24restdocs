# Galleries

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A gallery opens the images of a block in a separate viewer: the visitor clicks an image and sees it at full size, with navigation to the neighbouring images. The markup stays regular — cards and image nodes.

The scenario suits blocks with a portfolio, a photo report, or a product showcase. If the images have to be scrolled right inside the block, use [Sliders](./sliders.md).

The behavior is enabled by the `landing_gallery_cards` extension, which is connected in the [block manifest](../manifest.md).

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

## What the `landing_gallery_cards` Extension Does

The extension prepares the images inside `js-gallery-cards` for viewing in gallery mode and creates a clickable `<a>` wrapper. The entire content of the image's parent is moved into the wrapper, not just the `<img>` tag itself. The viewer is built by the fancybox library, which Bitrix24 connects together with the extension.

When opened, the image from `src` is used. If the node has a `srcset`, it is also passed to the viewing settings.

The images are scrolled within the group defined by the `data-fancybox` value. The extension appends the block identifier to that value, so identical values in different blocks do not mix. If two independent galleries are needed within one block, give the images different values of the attribute.

## Gallery Markup

The root container of the gallery has to have the class `js-gallery-cards`. The extension needs nothing else from the container. An image has to have the `data-fancybox` attribute and a filled-in source: without an image address, the wrapper link is not created.

Classes such as `landing-block-node-card` and `landing-block-node-card-img` in the examples below are the selectors of a standard block: the manifest uses them to link the markup with cards and nodes. In standard gallery blocks, every image is described as a card, so the composition of such a gallery is changed by the card methods, while the image itself is an [`img` node](../node-types.md). This is not mandatory in your own block: if the images are not described in the `cards` key of the manifest, the card methods do not apply to them, and the gallery still works.

Image attributes:

#|
|| **Attribute** | **Value** | **What It Defines** ||
|| `data-fancybox` | The name of the image group, `gallery` for example | Participation of the image in the gallery. Without the attribute, the image does not open in the viewer ||
|| `src` | The image address | The source that opens on click. Without it, the wrapper link is not created ||
|| `data-link-classes` | A string of CSS classes, `d-block g-pos-rel` for example | Classes for the wrapper link around the image. An optional attribute ||
|| `data-lazy-img` | `Y` | Lazy loading mode: the source is taken from `data-src` and `data-srcset` instead of `src` and `srcset` ||
|| `data-src` | The image address | The source in lazy loading mode. Works only together with `data-lazy-img="Y"` ||
|| `data-srcset` | A set of addresses, as in the `srcset` attribute | Image variants for different screen densities in lazy loading mode ||
|| `alt` | A string | The caption displayed under the image in the viewer ||
|#

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

## Examples of Standard Blocks

Codes for some standard blocks:

- `32.5.img_grid_3cols_1`
- `32.6.img_grid_4cols_1`
- `45.1.gallery_app_wo_slider`
- `45.2.gallery_app_with_slider` — a gallery together with a slider
- `45.3.gallery_6cols_2row` — also with a slider

## How to Change the Images via REST

1. Upload the file using the [landing.block.uploadfile](../methods/landing-block-upload-file.md) method.
2. Substitute the resulting `src` into the image node using the [landing.block.updatenodes](../methods/landing-block-update-nodes.md) method.
3. To add or remove a gallery card, use the [landing.block.addcard](../methods/landing-block-add-card.md), [landing.block.clonecard](../methods/landing-block-clone-card.md), and [landing.block.removecard](../methods/landing-block-remove-card.md) methods.

## Permissions and Limitations

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

Limitations:

- the extension processes only containers with the `js-gallery-cards` class and images with the `data-fancybox` attribute. A custom container class cannot be set
- the viewer opens the image from `src`. There is no separate address for a full-size version
- in edit mode, the gallery is forcibly disabled, so the result is checked in the preview or on the published page

## Continue Your Exploration

- [{#T}](./index.md)
- [{#T}](./sliders.md)
- [{#T}](./timer.md)
- [{#T}](../manifest.md)
- [{#T}](../node-types.md)