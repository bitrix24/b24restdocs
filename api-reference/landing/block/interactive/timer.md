# Countdown Timers

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet writing standards

{% endnote %}

{% endif %}

In the [block manifest](../manifest.md), add the extension `landing_countdown`.

```php
'assets' => array(
    'ext' => array('landing_countdown'),
),
```

In the **block** section of the manifest, add the key:

```php
'version' => '18.5.0'
```

The version parameter is optional, but it will limit the addition of this block in versions earlier than specified, when the necessary assets did not yet exist.

Add attributes for the countdown container node to the node that corresponds to the description and should act as the timer. For details, see the example.

```php
'attrs' => array(
    '.landing-block-node-date' => array(
        'name' => Loc::getMessage('LANDING_BLOCK_51_2_COUNTDOWN_04--DATE'),
        'time' => true,
        'type' => 'date',
        'format' => 'ms',
        'attribute' => 'data-end-date',
    ),
),
```

## Markup

The timer should contain 4 numeric elements, marked with the corresponding classes:

- **js-cd-days** - days
- **js-cd-hours** - hours
- **js-cd-minutes** - minutes
- **js-cd-seconds** - seconds

The ability to add a year is not available; we consider it impractical to create such long timers on the site.

Wrap the numbers in a common container marked with the class **js-countdown**. Pass the settings to this container using data attributes.

- **data-end-date="1586690955000"** - the end date of the timer in Unix-time format in milliseconds. That is, the obtained Unix date should be multiplied by 1000.
- **data-days-format="%D"** - format for displaying days
- **data-hours-format="%H"** - format for displaying hours
- **data-minutes-format="%M"** - format for displaying minutes
- **data-seconds-format="%S"** - format for displaying seconds

Two format options are available:
- **"%S"** - outputs the number with leading zeros "03", but "18",
- **"%-S"** - outputs only significant digits "3" or "18".

Instead of **"%H"**, you can use **"%I"** or **"%-I"**. This value represents the total number of hours remaining (i.e., 1 day 6 hours becomes 30 hours). In this case, you need to remove data-days-format and the node .js-cd-days.

Optional parameter:

```html
data-days-expired-classes="u-countdown--days-expiried"
```

When the number of days reaches zero, the timer can add a marking class. This will help you hide the zero days or highlight them in some way. We use the class **.u-countdown--days-hide** for this.

## Example

You can view examples of blocks of this type in our repository using the methods [landing.block.getmanifestfile](.) and [landing.block.getrepository](.). Their codes:

- 51.2.countdown_04
- 51.3.countdown_08
- 51.3.countdown_08_wo_bg
- 51.4.countdown_music
- 51.5.countdown_event
- 51.7.countdown_13
- 51.1.countdown_01

Example of a simple timer

```html
<section class="landing_block g-pt-30 g-pb-30 g-bg-orange g-color-white">
    <div class="landing-block-node-date mx-auto js-countdown text-center g-font-weight-300 g-line-height-1-2"
        data-end-date="1555249081000"
        data-days-format="%D"
        data-hours-format="%H"
        data-minutes-format="%M"
        data-seconds-format="%S"
        data-days-expired-classes="u-countdown--days-expiried"
    >

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

{% include [Footnote about examples](../../../../_includes/examples.md) %}