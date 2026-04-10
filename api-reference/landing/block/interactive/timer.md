# Countdown Timers

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

To implement a timer in the [block manifest](../manifest.md), connect the `landing_countdown` extension.

## How to Configure the Timer

The minimum configuration is as follows:

```php
'assets' => [
    'ext' => ['landing_countdown'],
],
```

Optionally, you can specify the block version:

```php
'block' => [
    'version' => '18.5.0',
],
```

The `version` is used to restrict the addition of the block in older versions of the product where the necessary resources are not yet available.

## What the Timer Extension Does

The `landing_countdown` extension calculates the remaining time until `data-end-date` and updates the values of the elements `js-cd-days`, `js-cd-hours`, `js-cd-minutes`, and `js-cd-seconds`.

## End Date Attribute

For the timer container node, add the end date attribute:

```php
'attrs' => [
    '.landing-block-node-date' => [
        [
            'name' => 'End Date',
            'type' => 'date',
            'time' => true,
            'format' => 'ms',
            'attribute' => 'data-end-date',
        ],
    ],
],
```

## Timer Markup

The timer container must have the class `js-countdown`. Inside the container, there should be nodes:

- `js-cd-days` — days
- `js-cd-hours` — hours
- `js-cd-minutes` — minutes
- `js-cd-seconds` — seconds

Optionally, you can add:

- `js-cd-years` — years
- `js-cd-month` — months

Supported attributes include:

- `data-end-date` — end date in Unix time in milliseconds
- `data-start-date` — start date for calculations
- `data-years-format` — format for displaying years
- `data-month-format` — format for displaying months
- `data-days-format` — format for displaying days
- `data-hours-format` — format for displaying hours
- `data-minutes-format` — format for displaying minutes
- `data-seconds-format` — format for displaying seconds
- `data-days-expired-classes` — classes added when the days value is zero

Value formats:

- `%S` — with leading zero, e.g., `03`
- `%-S` — without leading zero, e.g., `3`

For hours, you can use `%H` or `%I`/`%-I`. The `%I` and `%-I` formats display the total number of hours remaining until the timer ends. In this mode, it is common to remove `data-days-format` and the `js-cd-days` element.

## Example

```html
<section class="landing_block g-pt-30 g-pb-30 g-bg-orange g-color-white">
    <div class="landing-block-node-date mx-auto js-countdown text-center g-font-weight-300 g-line-height-1-2"
        data-end-date="1555249081000"
        data-days-format="%D"
        data-hours-format="%H"
        data-minutes-format="%M"
        data-seconds-format="%S"
        data-days-expired-classes="u-countdown--days-expiried">

        <div class="landing-block-node-number u-countdown--days-hide d-inline-block g-mx-20">
            <div class="landing-block-node-number-number g-font-size-36 mb-0">
                <span class="js-cd-days">12</span>
            </div>
        </div>

        <div class="landing-block-node-number-delimiter u-countdown--days-hide d-inline-block g-font-size-36">:</div>
    
        <div class="landing-block-node-number d-inline-block g-mx-20">
            <div class="landing-block-node-number-number g-font-size-36 mb-0">
                <span class="js-cd-hours">01</span>
            </div>
        </div>

        <div class="landing-block-node-number-delimiter d-inline-block g-font-size-36">:</div>

        <div class="landing-block-node-number d-inline-block g-mx-20">
            <div class="landing-block-node-number-number g-font-size-36 mb-0">
                <span class="js-cd-minutes">52</span>
            </div>
        </div>

        <div class="landing-block-node-number-delimiter d-inline-block g-font-size-36">:</div>

        <div class="landing-block-node-number d-inline-block g-mx-20">
            <div class="landing-block-node-number-number g-font-size-36 mb-0">
                <span class="js-cd-seconds">52</span>
            </div>
        </div>
    </div>
</section>
```

## Examples of Standard Blocks

Examples of blocks of this type can be viewed in the repository through the methods [landing.block.getmanifestfile](../methods/landing-block-get-manifest-file.md) and [landing.block.getrepository](../methods/landing-block-get-repository.md).

Codes for some standard blocks:

- `51.2.countdown_04`
- `51.3.countdown_08`
- `51.3.countdown_08_wo_bg`
- `51.4.countdown_music`
- `51.5.countdown_event`
- `51.7.countdown_13`
- `51.1.countdown_01`

## Important Considerations

- `data-end-date` is passed in milliseconds
- for `%I` and `%-I`, the total number of hours is displayed, without a separate days block
- if you need to hide days after they reach zero, use `data-days-expired-classes`

## Continue Learning

- [Sliders](./sliders.md)
- [Galleries](./gallery.md)
- [Block Manifest File](../manifest.md)
- [Node Types](../node-types.md)