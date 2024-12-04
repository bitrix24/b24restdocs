# Galleries

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed to meet writing standards

{% endnote %}

{% endif %}

The gallery is created using standard node images and cards. In the [block manifest](../manifest.md), specify the extension **landing_gallery_cards**.

```js
'assets' => array(
    'ext' => array('landing_gallery_cards'),
),
```

In the markup, designate the container with the class **.js-gallery-cards**, and inside it, add the required number of `<img>` nodes. Add the attribute **data-fancybox="gallery"** to each image. This attribute can have any value except for empty.

The gallery has only one version of the image, not a thumbnail and a full version as usual. Therefore, use images of sufficient size or scale them using browser tools (limit width/height). The gallery script will wrap each image in a link, and clicking will open the image specified in **src**.

Optionally, the attribute **data-link-classes="d-block g-pos-rel"** is allowed.

Both classes are added to the wrapper link around the image; they are necessary for layout.

Galleries can be combined with other features. For example, images can be cards, allowing the client to add as many as needed. Alternatively, it can be a slider, where each image can be opened in a larger size. Examples can be seen in our standard blocks.

{% note warning %}

When combining a gallery and a carousel (slider), you need to initialize assets in a specific order: first the carousel, then the gallery! Other assets, if present, can be in any sequence. See the code below.

{% endnote %}

```js
'assets' => [
    'ext' => ['landing_carousel', 'landing_gallery_cards'],
],
```

## Example

```html
<div class="landing-block g-pt-80 g-pb-80">
    <div class="container">
        <div class="js-gallery-cards row">
            <div class="landing-block-node-card js-animation slideInUp text-center col-lg-3 col-md-4 col-sm-6 g-mb-30">
                <div class="g-pos-rel d-inline-block">
                    <img class="landing-block-node-card-img h-100 g-width-auto g-max-width-100x g-max-height-350 g-max-height-500--md"
                        src="https://cdn.bitrix24.site/bitrix/images/landing/business/270x481/img1.jpg"
                        data-fancybox="gallery"
                        data-link-classes="d-block g-pos-rel" alt=""/>
                </div>
            </div>

            <div class="landing-block-node-card js-animation slideInUp text-center col-lg-3 col-md-4 col-sm-6 g-mb-30">
                <div class="g-pos-rel d-inline-block">
                    <img class="landing-block-node-card-img h-100 g-width-auto g-max-width-100x g-max-height-350 g-max-height-500--md"
                        src="https://cdn.bitrix24.site/bitrix/images/landing/business/270x481/img2.jpg"
                        data-fancybox="gallery"
                        data-link-classes="d-block g-pos-rel" alt=""/>
                </div>
            </div>

            <div class="landing-block-node-card js-animation slideInUp text-center col-lg-3 col-md-4 col-sm-6 g-mb-30">
                <div class="g-pos-rel d-inline-block">
                    <img class="landing-block-node-card-img h-100 g-width-auto g-max-width-100x g-max-height-350 g-max-height-500--md"
                        src="https://cdn.bitrix24.site/bitrix/images/landing/business/270x481/img3.jpg"
                        data-fancybox="gallery"
                        data-link-classes="d-block g-pos-rel" alt=""/>
                </div>
            </div>

            <div class="landing-block-node-card js-animation slideInUp text-center col-lg-3 col-md-4 col-sm-6 g-mb-30">
                <div class="g-pos-rel d-inline-block">
                    <img class="landing-block-node-card-img h-100 g-width-auto g-max-width-100x g-max-height-350 g-max-height-500--md"
                        src="https://cdn.bitrix24.site/bitrix/images/landing/business/270x481/img4.jpg"
                        data-fancybox="gallery"
                        data-link-classes="d-block g-pos-rel" alt=""/>
                </div>
            </div>

        </div>
    </div>
</div>
```

{% include [Footnote about examples](../../../../_includes/examples.md) %}