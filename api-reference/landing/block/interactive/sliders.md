# Sliders

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed to meet writing standards

{% endnote %}

{% endif %}

In the [block manifest](../manifest.md), include the extension `landing_carousel`.

```php
'assets' => array(
    'ext' => array('landing_carousel'),
)
```

In the block markup, mark the nodes with classes:

- **.js-carousel** - the root container of the slider
- **.js-slide** - each slide individually

By default, one slide is shown at a time, and each slide takes up the full width of the container. Navigation buttons and indicators are absent. Automatic scrolling is disabled. All of this can be changed with settings defined by data attributes. The attributes should be added to the **.js-carousel** element.

{% note warning %}

When combining a gallery and a carousel (slider), you need to initialize the assets in a specific order: first the carousel, then the gallery! Other assets, if present, can be in any order. See the code below.

{% endnote %}

```php
'assets' => [
    'ext' => ['landing_carousel', 'landing_gallery_cards'],
],
```

## Attributes

**Navigation buttons.**

This attribute adds navigation buttons. Styles are defined as common for both buttons, as well as separately for the left and right ones.

```html
data-arrows-classes="u-arrow-v1 g-absolute-centered--y g-width-45 g-height-45 g-color-white g-bg-primary"
data-arrow-left-classes="fa fa-chevron-left g-left-0"
data-arrow-right-classes="fa fa-chevron-right g-right-0"
```

**Page indicators (pagination)**

This attribute adds a pagination element and sets its classes.

```html
data-pagi-classes="u-carousel-indicators-v1 g-absolute-centered--x g-bottom-60 text-center"
```

**Number of slides on the screen**

```html
data-slides-show="3"
```

**Number of slides that change with one swipe**

```html
data-slides-scroll="2"
```

**Enable/disable autoplay**

```html
data-autoplay="true"
```

**Autoplay speed in milliseconds**

```html
data-speed="1000"
```

**Stop autoplay on mouse hover**

```html
data-pause-hover="true"
```

**Slide "fade" effect**

This attribute allows slides to change places with a change in opacity instead of swiping.

```html
data-fade="true"
```
{% note warning %}

Works correctly only with data-slides-show="1"

{% endnote %}

**Vertical slider**

```html
data-vertical="true"
```

Be careful, the buttons and pagination for vertical sliders must differ from horizontal ones (they should be positioned differently). Examples can be seen in the standard blocks. It is recommended to disable verticality on mobile devices using the **Responsiveness** setting. Otherwise, scrolling with a finger on the screen will not move the page but will swipe the slides.

**Number of rows**

```html
data-rows="2"
```

In a multi-row slider, the parameters **data-slides-show** and **data-slides-scroll** affect the number of columns, not the number of slides.

**Loop playback**

If enabled, after the last slide, the first one will be shown again. If disabled, playback will stop. For compatibility with the editor, this setting works only in Preview and Publish mode. In the editor, looping is always disabled.

```html
data-infinite="true"
```

**Responsiveness**

Sliders can flexibly change their settings depending on the screen size. Any of the above settings can be subject to responsiveness, but the **Number of slides on the screen** is most often changed.

In the attribute, you need to pass an array of objects, each of which should contain:

- **breakpoint** - screen size in pixels. The rule applies "downwards," meaning for screens of this size and smaller.
- **settings** - an array of settings applied for this rule. The names of the settings differ from the names of the data attributes. The list of names for the previously mentioned attributes:
  - **arrowsClasses**
  - **prevArrow**
  - **nextArrow**
  - **dotsClass**
  - **slidesToShow**
  - **slidesToScroll**
  - **autoplay**
  - **autoplaySpeed**
  - **pauseOnHover**
  - **fade**
  - **vertical**

```js
data-responsive='[{
    "breakpoint": 1200,
    "settings": {
        "slidesToShow": 5
    }
}, {
    "breakpoint": 992,
    "settings": {
        "slidesToShow": 3
    }
}, {
    "breakpoint": 768,
    "settings": {
        "slidesToShow": 2
    }
}, {
    "breakpoint": 576,
    "settings": {
        "slidesToShow": 1
    }
}]'
```

You can view examples of blocks of this type in our repository by using the methods [landing.block.getmanifestfile](../methods/landing-block-get-manifest-file.md) and [landing.block.getrepository](../methods/landing-block-get-repository.md). Their codes:

- 01.big_with_text
- 01.big_with_text_blocks
- 28.5.team_4_cols_slider
- 39.1.five_blocks_carousel
- 45.2.gallery_app_with_slider - with gallery
- and many others

A simple example:

```html
<div class="js-carousel"
    data-arrows-classes="u-arrow-v1 g-absolute-centered--y g-width-45 g-height-45 g-color-white g-bg-primary"
    data-arrow-left-classes="fa fa-chevron-left g-left-0"
    data-arrow-right-classes="fa fa-chevron-right g-right-0"
    data-pagi-classes="u-carousel-indicators-v1 g-absolute-centered--x g-bottom-60 text-center"
    data-slides-show="3"
    data-slides-scroll="2"
    data-autoplay="true"
    data-speed="1000"
    data-pause-hover="true"
    data-responsive='[
        {
            "breakpoint": 768,
            "settings": {
                "slidesToShow": 2
            }
        }, {
            "breakpoint": 576,
            "settings": {
                "slidesToShow": 1
            }
        }
    ]'
>

    <div class="js-slide g-height-50vh g-brd-gray-light-v3 g-brd-around g-bg-primary-opacity-0_1">
        <div class="g-flex-centered w-100 h-100">
            <h3>Slide 1</h3>
        </div>
    </div>

    <div class="js-slide g-height-50vh g-brd-gray-light-v3 g-brd-around g-bg-primary-opacity-0_1">
        <div class="g-flex-centered w-100 h-100">
            <h3>Slide 2</h3>
        </div>
    </div>

    <!-- ... and other slides ... -->

</div>
```

{% include [Footnote about examples](../../../../_includes/examples.md) %}