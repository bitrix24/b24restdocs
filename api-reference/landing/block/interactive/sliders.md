# Sliders

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

To enable the slider in the [block manifest](../manifest.md), include the `landing_carousel` extension.

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

## What the Slider Extension Does

The `landing_carousel` extension initializes the slider for the `js-carousel` container and processes the `data-*` attributes that control display, navigation, and autoplay.

## Markup

The markup uses two utility classes:

- `js-carousel` — the root container of the slider
- `js-slide` — an individual slide

By default, the slider displays one slide, without arrows, pagination, or autoplay. The behavior can be configured through `data-*` attributes on the `js-carousel` element.

## Key Attributes

- `data-arrows-classes` — classes for both arrows
- `data-arrow-left-classes` — classes for the left arrow
- `data-arrow-right-classes` — classes for the right arrow
- `data-pagi-classes` — classes for the pagination block
- `data-slides-show` — number of slides to show on the screen
- `data-slides-scroll` — number of slides to scroll at one time
- `data-autoplay` — enable/disable autoplay
- `data-speed` — autoplay speed in milliseconds
- `data-pause-hover` — stop autoplay on hover
- `data-fade` — fade effect for slide transitions, works correctly with `data-slides-show="1"`
- `data-vertical` — vertical slider mode
- `data-rows` — number of rows
- `data-infinite` — looping
- `data-responsive` — responsive rules based on breakpoints
- `data-center-mode` — center the active slide
- `data-center-padding` — padding on the edges in center mode
- `data-variable-width` — slides with variable width
- `data-initial-slide` — initial slide on load
- `data-rtl` — right-to-left display direction
- `data-adaptive-height` — adaptive height of the container based on the current slide
- `data-lazy-load` — lazy loading mode for images
- `data-nav-for` — link to another slider, e.g., a preview slider
- `data-is-thumbs` — thumbnail slider mode

For `data-responsive`, a valid JSON array of rules is provided. Each rule has:

- `breakpoint` — screen width in pixels
- `settings` — settings for that breakpoint

In `settings`, the parameter names from Slick are used instead of `data-*` attributes:

- `arrows` — show or hide arrows
- `prevArrow` — HTML/selector for the left arrow
- `nextArrow` — HTML/selector for the right arrow
- `dots` — enable or disable pagination
- `dotsClass` — CSS class for the pagination container
- `slidesToShow` — number of slides to show on the screen
- `slidesToScroll` — number of slides to scroll at one time
- `autoplay` — enable or disable autoplay
- `autoplaySpeed` — autoplay interval in milliseconds
- `pauseOnHover` — stop autoplay on hover
- `fade` — fade transition mode
- `vertical` — vertical slider mode

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

If the slider is used together with a gallery, include the extensions in the following order:

1. `landing_carousel`
2. `landing_gallery_cards`

```php
'assets' => [
    'ext' => ['landing_carousel', 'landing_gallery_cards'],
],
```

## Examples of Standard Blocks

Examples of this type of block can be viewed in the repository through the methods [landing.block.getmanifestfile](../methods/landing-block-get-manifest-file.md) and [landing.block.getrepository](../methods/landing-block-get-repository.md).

Codes for some standard blocks:

- `01.big_with_text`
- `01.big_with_text_blocks`
- `28.5.team_4_cols_slider`
- `39.1.five_blocks_carousel`
- `45.2.gallery_app_with_slider` — with a gallery

## Important Considerations

- `data-fade` works correctly with `data-slides-show="1"`
- In `data-rows` mode, the `data-slides-show` and `data-slides-scroll` parameters function as the number of columns
- For vertical mode, `data-vertical` usually requires separate configuration for arrows/pagination
- When `data-vertical` is enabled, `verticalSwiping` is also activated, so on mobile devices, vertical mode is typically disabled through `data-responsive`
- To display pagination, in addition to the dots settings, `data-pagi-classes` must be specified
- `data-infinite` works in preview and publication mode; in the editor, looping is forcibly disabled
- When combining with an embedded gallery, first include `landing_carousel`, then `landing_gallery_cards`

## Continue Learning

- [Galleries](./gallery.md)
- [Countdown Timers](./timer.md)
- [Block Manifest File](../manifest.md)
- [Node Types](../node-types.md)