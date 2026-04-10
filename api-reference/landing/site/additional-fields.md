# Additional Website Fields

Additional website fields are passed in the `ADDITIONAL_FIELDS` array when calling the methods [landing.site.add](./landing-site-add.md) and [landing.site.update](./landing-site-update.md).

Filled values can be retrieved using the method [landing.site.getadditionalfields](./landing-site-get-additional-fields.md).

{% note info "" %}

The method `landing.site.getadditionalfields` returns only non-empty website fields.

For recording, use the field codes in the `ADDITIONAL_FIELDS` array, for example:

```json
{
    "ADDITIONAL_FIELDS": {
        "THEME_CODE": "1construction",
        "BACKGROUND_USE": "Y",
        "BACKGROUND_COLOR": "#f4f7fb"
    }
}
```

{% endnote %}

## What You Need to Know

- The set of fields depends on the type of website, settings, and installed modules, so not all fields are available on every website.
- For flag fields, the values `Y` and `N` are used.
- If a field is empty, the method `landing.site.getadditionalfields` will not return it in the response.
- The field `BACKGROUND_PICTURE` stores the file identifier when recorded, and the REST method usually returns the file URL in the response.

## Theme and Fonts

#|
|| **Field**
`type` | **Description** ||
|| **THEME_CODE**
[`string`](../../data-types.md) | The color theme code of the website. Available values are listed in the section [Page Color Themes](../page/color-themes.md) ||
|| **THEME_USE**
[`string`](../../data-types.md) | Enable a custom color palette for the website: `Y` or `N` ||
|| **THEME_COLOR**
[`string`](../../data-types.md) | Custom theme color in HEX format, for example `#34bcf2` ||
|| **THEMEFONTS_USE**
[`string`](../../data-types.md) | Enable custom font settings for the website: `Y` or `N` ||
|| **THEMEFONTS_CODE**
[`string`](../../data-types.md) | Font for the main text ||
|| **THEMEFONTS_CODE_H**
[`string`](../../data-types.md) | Font for headings ||
|| **THEMEFONTS_SIZE**
[`string`](../../data-types.md) | Base text size. Supported values are `0.92857`, `1`, `1.14286` ||
|| **THEMEFONTS_COLOR**
[`string`](../../data-types.md) | Color of the main text in HEX format ||
|| **THEMEFONTS_COLOR_H**
[`string`](../../data-types.md) | Color of headings in HEX format ||
|| **THEMEFONTS_LINE_HEIGHT**
[`string`](../../data-types.md) | Line height. Supported values range from `0.7` to `2` ||
|| **THEMEFONTS_FONT_WEIGHT**
[`string`](../../data-types.md) | Font weight of the main text. Supported values range from `100` to `900` in increments of `100` ||
|| **THEMEFONTS_FONT_WEIGHT_H**
[`string`](../../data-types.md) | Font weight of headings. Supported values range from `100` to `900` in increments of `100` ||
|#

## Bitrix24 Widget

#|
|| **Field**
`type` | **Description** ||
|| **B24BUTTON_USE**
[`string`](../../data-types.md) | Enable the Bitrix24 widget: `Y` or `N` ||
|| **B24BUTTON_CODE**
[`string`](../../data-types.md) | Code or URL of the Bitrix24 widget script. It can be obtained in the widget settings in CRM ||
|| **B24BUTTON_COLOR**
[`string`](../../data-types.md) | Source of the widget color. Supported values are `site`, `button`, `custom` ||
|| **B24BUTTON_COLOR_VALUE**
[`string`](../../data-types.md) | Custom widget color in HEX format. Used if `B24BUTTON_COLOR = custom` ||
|#

## Back to Top Button

#|
|| **Field**
`type` | **Description** ||
|| **UP_SHOW**
[`string`](../../data-types.md) | Show the back to top button: `Y` or `N` ||
|#

## Website Background

#|
|| **Field**
`type` | **Description** ||
|| **BACKGROUND_USE**
[`string`](../../data-types.md) | Enable website background settings: `Y` or `N` ||
|| **BACKGROUND_PICTURE**
[`string`](../../data-types.md) \| [`integer`](../../data-types.md) | Background image of the website. When recorded, the file identifier is passed; when read, the REST method usually returns the file URL ||
|| **BACKGROUND_POSITION**
[`string`](../../data-types.md) | Background display mode. Supported values are `center`, `repeat`, `center_repeat_y`, `no_repeat` ||
|| **BACKGROUND_COLOR**
[`string`](../../data-types.md) | Background color of the website in HEX format ||
|#

## Analytics

Analytics fields are not available on all plans. If analytics integrations are not available on your plan, enabling these fields will not activate counters.

#|
|| **Field**
`type` | **Description** ||
|| **GACOUNTER_USE**
[`string`](../../data-types.md) | Enable Google Analytics: `Y` or `N` ||
|| **GACOUNTER_COUNTER**
[`string`](../../data-types.md) | Identifier of the old format counter `UA-...`. For the current GA4 counter, use the field `GACOUNTER_COUNTER_GA4` ||
|| **GACOUNTER_COUNTER_GA4**
[`string`](../../data-types.md) | Identifier of the Google Analytics 4 counter in the format `G-...` ||
|| **GACOUNTER_SEND_CLICK**
[`string`](../../data-types.md) | Send click events: `Y` or `N` ||
|| **GACOUNTER_CLICK_TYPE**
[`string`](../../data-types.md) | Where to get the signature for the click event. Supported values are `href` and `text` ||
|| **GACOUNTER_SEND_SHOW**
[`string`](../../data-types.md) | Send block show events: `Y` or `N` ||
|| **GTM_USE**
[`string`](../../data-types.md) | Enable Google Tag Manager: `Y` or `N` ||
|| **GTM_COUNTER**
[`string`](../../data-types.md) | Identifier of the Google Tag Manager container ||
|#

## Maps

#|
|| **Field**
`type` | **Description** ||
|| **GMAP_USE**
[`string`](../../data-types.md) | Enable Google Maps API: `Y` or `N` ||
|| **GMAP_CODE**
[`string`](../../data-types.md) | Google Maps API key. Maps are connected only if `GMAP_USE = Y` and this field is filled ||
|#

## Website View

#|
|| **Field**
`type` | **Description** ||
|| **VIEW_USE**
[`string`](../../data-types.md) | Enable special website view: `Y` or `N` ||
|| **VIEW_TYPE**
[`string`](../../data-types.md) | Type of view. Supported values are `no`, `ltr`, `all`, `mobile`, `adaptive` ||
|#

## Robots.txt

#|
|| **Field**
`type` | **Description** ||
|| **ROBOTS_USE**
[`string`](../../data-types.md) | Enable custom `robots.txt`: `Y` or `N` ||
|| **ROBOTS_CONTENT**
[`string`](../../data-types.md) | Content of the custom `robots.txt` ||
|#

## Custom Code

The fields `HEADBLOCK_*` are not available on all plans.

#|
|| **Field**
`type` | **Description** ||
|| **HEADBLOCK_USE**
[`string`](../../data-types.md) | Enable custom HTML or JavaScript in `head`: `Y` or `N` ||
|| **HEADBLOCK_CODE**
[`string`](../../data-types.md) | Arbitrary HTML or JavaScript added to the website's `head` ||
|| **CSSBLOCK_USE**
[`string`](../../data-types.md) | Enable custom CSS: `Y` or `N` ||
|| **CSSBLOCK_CODE**
[`string`](../../data-types.md) | Arbitrary CSS code for the website ||
|| **CSSBLOCK_FILE**
[`string`](../../data-types.md) | Link to an external CSS file ||
|#

## Continue Learning

- [landing.site.add](./landing-site-add.md)
- [landing.site.update](./landing-site-update.md)
- [landing.site.getadditionalfields](./landing-site-get-additional-fields.md)
- [Website Fields](./base-fields.md)
- [Page Color Themes](../page/color-themes.md)