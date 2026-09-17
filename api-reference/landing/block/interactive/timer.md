# Countdown Timers

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A countdown timer displays on the page how much time is left until a given date: until the end of a promotion, the start of a webinar, or the close of a pre-order. The values are recalculated in the visitor's browser, with no requests to Bitrix24.

The scenario suits blocks with a time-limited offer. If the date is not fixed or the countdown has to start from a visitor's action, the timer will not fit: it can only count down to a specific moment.

The behavior is enabled by the `landing_countdown` extension, which is connected in the [block manifest](../manifest.md).

## How to Configure the Timer

The minimum configuration is as follows:

```php
'assets' => [
    'ext' => ['landing_countdown'],
],
```

The other manifest keys of such a block are the regular ones, and they are described in the [Block Manifest](../manifest.md) article.

## What the `landing_countdown` Extension Does

The extension finds the elements with the `js-countdown` class on the page, calculates the time remaining until `data-end-date`, and updates the values of the service elements inside the container. The values are redrawn once per second.

When no time is left until the end date, the countdown stops at zeros: it does not go negative.

Separately, the extension tracks the last day of the countdown: this is controlled by the `data-days-expired-classes` attribute from the table below.

## How to Describe the End Date in the Manifest

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

The timer container has to have the class `js-countdown`. Inside the container, elements for the time units are placed — only those that are displayed on the page are needed:

- `js-cd-years` — years
- `js-cd-month` — months
- `js-cd-days` — days
- `js-cd-hours` — hours
- `js-cd-minutes` — minutes
- `js-cd-seconds` — seconds

## HTML Attributes of the Container

The attributes are set on the `js-countdown` container.

#|
|| **Attribute** | **Value** | **What It Defines** ||
|| `data-end-date` | A number or a date string | The end date of the countdown. A number is Unix time in milliseconds, as with `format` set to `ms` in the manifest. A string is parsed by the browser's `Date` constructor, so pass it in the ISO 8601 format, `2026-12-31T23:59:59` for example. A string without an offset is treated as the visitor's local time, not as Bitrix24 time ||
|| `data-years-format` | A format template | The display of years ||
|| `data-month-format` | A format template | The display of months ||
|| `data-days-format` | A format template | The display of days ||
|| `data-hours-format` | A format template | The display of hours ||
|| `data-minutes-format` | A format template | The display of minutes ||
|| `data-seconds-format` | A format template | The display of seconds ||
|| `data-days-expired-classes` | A string of CSS classes | Classes added during the last day of the countdown, when the day counter reaches zero. This is how the days block is hidden ||
|#

In a format template, a letter denotes a time unit. Case matters: `%m` is months, while `%M` is minutes.

#|
|| **Directive** | **What It Displays** ||
|| `%Y` | Years. The difference between calendar years is counted, not the number of full years elapsed ||
|| `%m` | Full months ||
|| `%D` | Total days until the end of the countdown ||
|| `%d` | Days remaining within the week ||
|| `%H` | Hours remaining within the day ||
|| `%I` | Total hours until the end of the countdown ||
|| `%M` | Minutes remaining within the hour ||
|| `%S` | Seconds remaining within the minute ||
|| `%n` | Days remaining within the month ||
|| `%w` | Weeks ||
|| `%W` | Weeks remaining within the month ||
|| `%N` | Total minutes until the end of the countdown ||
|| `%T` | Total seconds until the end of the countdown ||
|#

By default, the value is padded with a leading zero to two digits: `03`. A hyphen after the percent sign removes that zero: `%-S` displays `3`. An exclamation mark enables inflection of the label: `%!d:day,days;` substitutes the appropriate form of the word.

The `%I` and `%-I` formats display the total number of hours, so `data-days-format` and the `js-cd-days` element are usually removed together with them.

## Example

```html
<section class="landing_block g-pt-30 g-pb-30 g-bg-orange g-color-white">
    <div class="landing-block-node-date mx-auto js-countdown text-center g-font-weight-300 g-line-height-1-2"
        data-end-date="1798761600000"
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

Codes for some standard blocks:

- `51.1.countdown_01`
- `51.2.countdown_04`
- `51.3.countdown_08`
- `51.3.countdown_08_wo_bg`
- `51.4.countdown_music`
- `51.5.countdown_event`
- `51.7.countdown_13`

## How to Change the End Date via REST

The date is retained in the `data-end-date` HTML attribute of the timer node, so it is modified by the [landing.block.updateattrs](../methods/landing-block-update-attrs.md) method. The attribute has to be described in the [attrs](../attributes.md) key of the block manifest: the method takes the node selector and the attribute name from there.

You can check the current value using the [landing.block.getcontent](../methods/landing-block-get-content.md) method with the `editMode = true` parameter: without it, the method returns the published version of the block, where the new date has not appeared yet.

## Permissions and Limitations

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

Limitations:

- a custom container class cannot be set: the extension looks for `js-countdown`
- if the clock on the visitor's device is off, the countdown will be inaccurate
- the timer is reinitialized after a block card is added. The extension is not subscribed to card modification and removal
- the `data-start-date` attribute does not affect the output: the calculation always runs from the current time to `data-end-date`

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sliders.md)
- [{#T}](./gallery.md)
- [{#T}](../manifest.md)
- [{#T}](../node-types.md)