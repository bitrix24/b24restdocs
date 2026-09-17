# Sliders

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A slider scrolls the content of a block right on the page: covers, reviews, product cards, or team photos. The visitor switches slides with arrows, pagination, or a swipe, while the block takes up a single screen.

The scenario suits blocks with many elements and limited space. If the images have to be opened at full size, use [Galleries](./gallery.md).

The behavior is enabled by the `landing_carousel` extension, which is connected in the [block manifest](../manifest.md).

## How to Configure the Slider

The minimal configuration is as follows:

```php
'assets' => [
    'ext' => ['landing_carousel'],
],
```

```html
<div class="js-carousel">
    <div class="js-slide">Slide 1</div>
    <div class="js-slide">Slide 2</div>
</div>
```

## What the `landing_carousel` Extension Does

The extension finds containers with the `js-carousel` class, assembles a slider from their direct children, and reads the settings from the `data-*` attributes of the container. The extension defines no separate slide selector: every direct child of the container becomes a slide.

The extension adds the arrows and the pagination itself: they are not present in the block markup. They appear only if the design classes are set — `data-arrows-classes` for the arrows and `data-pagi-classes` for the pagination. Without these attributes, the controls are not created: setting only `data-arrow-left-classes` and `data-arrow-right-classes` is not enough.

The slider is built on the Slick library, so `data-responsive` uses the names of its parameters rather than the names of the `data-*` attributes.

## Markup

The markup uses two utility classes:

- `js-carousel` — the root container of the slider
- `js-slide` — an individual slide. This class is needed for design: the styles of the standard blocks are connected through it. It does not affect how slides are found

By default, the slider displays one slide, without arrows, pagination, or autoplay. The behavior can be configured through `data-*` attributes on the `js-carousel` element.

## Key Attributes

Boolean attributes accept the values `true` and `false`.

#|
|| **Attribute** | **Value** | **What It Defines** ||
|| `data-slides-show` | A number | The number of slides on the screen. `1` by default ||
|| `data-slides-scroll` | A number | The number of slides per step ||
|| `data-initial-slide` | A number | The slide displayed on load. Counting starts from one: `1` is the first slide ||
|| `data-rows` | A number | The number of slider rows ||
|| `data-infinite` | `true`, `false` | Looping of the slides ||
|| `data-autoplay` | `true`, `false` | Autoplay ||
|| `data-speed` | A number | The autoplay interval in milliseconds. `3000` by default ||
|| `data-pause-hover` | `true`, `false` | Stopping autoplay on hover ||
|| `data-fade` | `true`, `false` | Slide transitions through opacity. Works correctly with `data-slides-show="1"` ||
|| `data-vertical` | `true`, `false` | Vertical slider mode ||
|| `data-adaptive-height` | `true`, `false` | Adapting the container height to the current slide ||
|| `data-center-mode` | `true`, `false` | Centering the active slide ||
|| `data-center-padding` | A size with a unit, `40px` for example | Padding on the edges in center mode ||
|| `data-variable-width` | `true`, `false` | Slides of variable width ||
|| `data-rtl` | `true`, `false` | Right-to-left display ||
|| `data-lazy-load` | A loading mode, `ondemand` for example | Lazy loading of images ||
|| `data-arrows-classes` | A string of CSS classes | Classes for both arrows. Without this attribute, the arrows are not created ||
|| `data-arrow-left-classes` | A string of CSS classes | Classes for the left arrow ||
|| `data-arrow-right-classes` | A string of CSS classes | Classes for the right arrow ||
|| `data-pagi-classes` | A string of CSS classes | Classes for the pagination block. Without this attribute, the pagination is not displayed ||
|| `data-nav-for` | A CSS selector of another slider | A link between two sliders, with a preview strip for example ||
|| `data-is-thumbs` | `true`, `false` | Preview slider mode for pairing with `data-nav-for`. Works only if the container has the `id` attribute set ||
|| `data-responsive` | A JSON array of rules | Settings for individual breakpoints ||
|#

## Responsive Rules in `data-responsive`

A valid JSON array is passed in `data-responsive`. Each rule has two keys:

- `breakpoint` — the screen width in pixels
- `settings` — the settings for that breakpoint

Inside `settings`, the Slick parameter names are used:

#|
|| **Parameter in `settings`** | **Counterpart Among `data-*`** | **What It Defines** ||
|| `slidesToShow` | `data-slides-show` | The number of slides on the screen ||
|| `slidesToScroll` | `data-slides-scroll` | The number of slides per step ||
|| `autoplay` | `data-autoplay` | Autoplay ||
|| `autoplaySpeed` | `data-speed` | The autoplay interval in milliseconds ||
|| `pauseOnHover` | `data-pause-hover` | Stopping autoplay on hover ||
|| `fade` | `data-fade` | Slide transitions through opacity ||
|| `vertical` | `data-vertical` | Vertical slider mode ||
|| `arrows` | — | Displaying the arrows. At the top level, the arrows are enabled with the `data-arrows-classes` classes ||
|| `prevArrow`, `nextArrow` | `data-arrow-left-classes`, `data-arrow-right-classes` | The markup or the selector of the arrows ||
|| `dots` | — | Displaying the pagination ||
|| `dotsClass` | `data-pagi-classes` | The CSS class of the pagination container. The extension always adds the service class `js-pagination` to the value from `data-pagi-classes` ||
|#

## Example

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
    data-center-mode="true"
    data-center-padding="40px"
    data-initial-slide="1"
    data-adaptive-height="true"
    data-lazy-load="ondemand"
    data-responsive='[
        {
            "breakpoint": 768,
            "settings": {
                "slidesToShow": 2
            }
        },
        {
            "breakpoint": 576,
            "settings": {
                "slidesToShow": 1
            }
        }
    ]'>

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
</div>
```

## Combining with a Gallery

A slider works together with a gallery: the images are scrolled inside the block and opened in the viewer. The order in which the extensions are connected matters, and it is described in the [Galleries](./gallery.md) article.

## Examples of Standard Blocks

Codes for some standard blocks:

- `01.big_with_text`
- `01.big_with_text_blocks`
- `28.5.team_4_cols_slider`
- `39.1.five_blocks_carousel`
- `45.2.gallery_app_with_slider` — with a gallery

## How to Change the Slider Settings via REST

The behavior of a slider is defined by the `data-*` attributes of the container, so their values are modified by the [landing.block.updateattrs](../methods/landing-block-update-attrs.md) method. Only the attributes described in the [attrs](../attributes.md) key of the block manifest are available.

The slides are the cards of the block, so their composition is changed by the [landing.block.addcard](../methods/landing-block-add-card.md), [landing.block.clonecard](../methods/landing-block-clone-card.md), [landing.block.removecard](../methods/landing-block-remove-card.md), and [landing.block.updateCards](../methods/landing-block-update-cards.md) methods, while the content of a slide is changed by [landing.block.updatenodes](../methods/landing-block-update-nodes.md).

## Permissions and Limitations

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

Limitations:

- a custom container class cannot be set: the extension looks for `js-carousel`
- in the editor, looping is forcibly disabled, so `data-infinite` is checked in the preview or on the published page
- when `data-vertical` is enabled, vertical swiping works at the same time, so for mobile devices vertical mode is usually disabled with a rule in `data-responsive`

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./gallery.md)
- [{#T}](./timer.md)
- [{#T}](../manifest.md)
- [{#T}](../node-types.md)