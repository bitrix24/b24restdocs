# Additional Page Fields

Additional page fields are passed in the `ADDITIONAL_FIELDS` array when calling the methods [landing.landing.add](./methods/landing-landing-add.md) and [landing.landing.update](./methods/landing-landing-update.md).

Filled values can be retrieved using the method [landing.landing.getadditionalfields](./methods/landing-landing-get-additional-fields.md).

{% note info "" %}

The method `landing.landing.getadditionalfields` returns only non-empty page fields.

For writing, use the field codes in the `ADDITIONAL_FIELDS` array, for example:

```json
{
    "ADDITIONAL_FIELDS": {
        "METAROBOTS_INDEX": "Y",
        "METAMAIN_USE": "Y",
        "METAMAIN_TITLE": "Page Title"
    }
}
```

{% endnote %}

## What You Need to Know

- The set of fields depends on the type of site, settings, and installed modules, so not all fields are available on every site.
- System fields may appear in the response of the reading method, but they are not set manually.
- For flag fields, use values `Y` and `N`.
- If a field is empty, the method `landing.landing.getadditionalfields` will not return it in the response.
- The fields `METAOG_IMAGE`, `BACKGROUND_PICTURE`, and `FAVICON_PICTURE` are related to files.
- The fields `SETTINGS_*` pertain to catalog and trading section settings.

## Theme and Fonts

#|
|| **Field**
`type` | **Description** ||
|| **THEME_CODE**
[`string`](../../data-types.md) | The color theme code of the page. Available values are listed in the section [Page Color Themes](./color-themes.md) ||
|| **THEME_USE**
[`string`](../../data-types.md) | Enable a custom color palette for the page: `Y` or `N` ||
|| **THEME_COLOR**
[`string`](../../data-types.md) | Custom theme color in HEX format, for example `#34bcf2` ||
|| **THEMEFONTS_USE**
[`string`](../../data-types.md) | Enable custom font settings for the page: `Y` or `N` ||
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
[`string`](../../data-types.md) | Font weight of the main text. Supported values range from `100` to `900` ||
|| **THEMEFONTS_FONT_WEIGHT_H**
[`string`](../../data-types.md) | Font weight of headings. Supported values range from `100` to `900` ||
|| **FONTS_CODE**
[`string`](../../data-types.md) | System field with HTML for connecting fonts. May be present in the response of the reading method ||
|#

## Social Networks and SEO

#|
|| **Field**
`type` | **Description** ||
|| **METAOG_TITLE**
[`string`](../../data-types.md) | Title for Open Graph, tag `og:title` ||
|| **METAOG_DESCRIPTION**
[`string`](../../data-types.md) | Description for Open Graph, tag `og:description` ||
|| **METAOG_IMAGE**
[`string`](../../data-types.md) | Image for Open Graph, tag `og:image`. Usually, the reading method returns the file URL ||
|| **METAMAIN_USE**
[`string`](../../data-types.md) | Enable custom meta tags for the page: `Y` or `N` ||
|| **METAMAIN_TITLE**
[`string`](../../data-types.md) | Value of the `title` tag ||
|| **METAMAIN_DESCRIPTION**
[`string`](../../data-types.md) | Value of the `description` tag ||
|| **METAMAIN_KEYWORDS**
[`string`](../../data-types.md) | Value of the `keywords` tag ||
|| **METAKEYWORDS_KEYWORDS**
[`string`](../../data-types.md) | Value of the `keywords` tag from a separate hook `METAKEYWORDS` ||
|| **METAROBOTS_INDEX**
[`string`](../../data-types.md) | Allow indexing of the page: `Y` or `N` ||
|| **METAGOOGLEVERIFICATION_USE**
[`string`](../../data-types.md) | Enable Google Search Console verification meta tag: `Y` or `N` ||
|| **METAGOOGLEVERIFICATION_META**
[`string`](../../data-types.md) | Full Google Search Console verification tag ||
|| **ROBOTS_USE**
[`string`](../../data-types.md) | Enable custom `robots.txt`: `Y` or `N` ||
|| **ROBOTS_CONTENT**
[`string`](../../data-types.md) | Content of the custom `robots.txt` ||
|#

## Background, Animation, Icon, and Page View

#|
|| **Field**
`type` | **Description** ||
|| **BACKGROUND_USE**
[`string`](../../data-types.md) | Enable background settings for the page: `Y` or `N` ||
|| **BACKGROUND_PICTURE**
[`string`](../../data-types.md) | Background image of the page. Usually, the reading method returns the file URL ||
|| **BACKGROUND_POSITION**
[`string`](../../data-types.md) | Background display mode. Supported values are `center`, `repeat`, `center_repeat_y`, `no_repeat` ||
|| **BACKGROUND_COLOR**
[`string`](../../data-types.md) | Background color of the page in HEX format ||
|| **FAVICON_PICTURE**
[`string`](../../data-types.md) | Favicon image. Usually, the reading method returns the file URL ||
|| **VIEW_USE**
[`string`](../../data-types.md) | Enable special page view: `Y` or `N` ||
|| **VIEW_TYPE**
[`string`](../../data-types.md) | Type of view. Supported values are `no`, `ltr`, `all`, `mobile`, `adaptive` ||
|| **PADDING_TOPBOTTOM**
[`string`](../../data-types.md) | Vertical outer margins of the page. Supported values are `N`, `u-outer-space-v1`, `u-outer-space-v2` ||
|| **PADDING_LEFTRIGHT**
[`string`](../../data-types.md) | Horizontal layout constraints. Supported values are `N`, `g-layout-semiboxed`, `g-layout-boxed` ||
|| **LAYOUT_BREAKPOINT**
[`string`](../../data-types.md) | Breakpoint for adaptive view. Supported values are `mobile`, `tablet`, `desktop`, `all` ||
|| **TRANSITION_COLOR**
[`string`](../../data-types.md) | Background color for the page appearance animation ||
|| **UP_SHOW**
[`string`](../../data-types.md) | Show back-to-top button: `Y` or `N` ||
|#

## Analytics and Pixels

#|
|| **Field**
`type` | **Description** ||
|| **GACOUNTER_USE**
[`string`](../../data-types.md) | Enable Google Analytics: `Y` or `N` ||
|| **GACOUNTER_COUNTER**
[`string`](../../data-types.md) | Identifier of the Universal Analytics counter ||
|| **GACOUNTER_COUNTER_GA4**
[`string`](../../data-types.md) | Identifier of the Google Analytics 4 counter ||
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
|| **PIXELFB_USE**
[`string`](../../data-types.md) | Enable Facebook pixel: `Y` or `N` ||
|| **PIXELFB_COUNTER**
[`string`](../../data-types.md) | Identifier of the Facebook pixel ||
|| **PIXELVK_USE**
[`string`](../../data-types.md) | Enable VK pixel: `Y` or `N` ||
|| **PIXELVK_COUNTER**
[`string`](../../data-types.md) | Identifier of the VK pixel ||
|#

## Custom Code and Styles

#|
|| **Field**
`type` | **Description** ||
|| **HEADBLOCK_USE**
[`string`](../../data-types.md) | Enable custom HTML or JavaScript in `head`: `Y` or `N` ||
|| **HEADBLOCK_CODE**
[`string`](../../data-types.md) | Arbitrary HTML or JavaScript added to the `head` of the page ||
|| **CSSBLOCK_USE**
[`string`](../../data-types.md) | Enable custom CSS: `Y` or `N` ||
|| **CSSBLOCK_CODE**
[`string`](../../data-types.md) | Arbitrary CSS code for the page ||
|| **CSSBLOCK_FILE**
[`string`](../../data-types.md) | Link to an external CSS file ||
|| **CUSTOMCSS_CSS_CODE**
[`string`](../../data-types.md) | Additional custom CSS code from a separate hook ||
|| **CUSTOMCSS_CSS_URL**
[`string`](../../data-types.md) | Link to an external CSS file from a separate hook ||
|#

## Maps

#|
|| **Field**
`type` | **Description** ||
|| **GMAP_USE**
[`string`](../../data-types.md) | Enable Google Maps API: `Y` or `N` ||
|| **GMAP_CODE**
[`string`](../../data-types.md) | Google Maps API key ||
|#

## Bitrix24 Widget

#|
|| **Field**
`type` | **Description** ||
|| **B24BUTTON_USE**
[`string`](../../data-types.md) | Enable Bitrix24 widget: `Y` or `N` ||
|| **B24BUTTON_CODE**
[`string`](../../data-types.md) | Code or URL of the widget script ||
|| **B24BUTTON_COLOR**
[`string`](../../data-types.md) | Source of the widget color. Supported values are `site`, `button`, `custom` ||
|| **B24BUTTON_COLOR_VALUE**
[`string`](../../data-types.md) | Custom widget color in HEX format ||
|| **B24BUTTON_HELP**
[`string`](../../data-types.md) | System field with a link to help ||
|#

## Cookies and System Banners

#|
|| **Field**
`type` | **Description** ||
|| **COOKIES_USE**
[`string`](../../data-types.md) | Show cookies banner: `Y` or `N` ||
|| **COOKIES_AGREEMENT_ID**
[`string`](../../data-types.md) | Use agreement in the cookies banner: `Y` or `N` ||
|| **COOKIES_COLOR_BG**
[`string`](../../data-types.md) | Background color of the cookies banner in HEX format ||
|| **COOKIES_COLOR_TEXT**
[`string`](../../data-types.md) | Text color of the cookies banner in HEX format ||
|| **COOKIES_POSITION**
[`string`](../../data-types.md) | Position of the cookies banner. Supported values are `bottom_left`, `bottom_right` ||
|| **COOKIES_MODE**
[`string`](../../data-types.md) | Mode of the cookies banner. Supported values are `A` and `I` ||
|| **COPYRIGHT_SHOW**
[`string`](../../data-types.md) | Show copyright or system footer: `Y` or `N` ||
|| **COPYRIGHT_CODE**
[`string`](../../data-types.md) \| [`integer`](../../data-types.md) | Identifier of the text variant for the footer ||
|#

## Performance

#|
|| **Field**
`type` | **Description** ||
|| **SPEED_ASSETS**
[`string`](../../data-types.md) | System field with a list of resources for optimization ||
|| **SPEED_USE_LAZY**
[`string`](../../data-types.md) | Use lazy load: `Y` or `N` ||
|| **SPEED_USE_WEBPACK**
[`string`](../../data-types.md) | Use webpack resource build mode: `Y` or `N` ||
|#

## Catalog and Trading Section Settings

These fields are generated by the `SETTINGS` hook. They depend on the installed catalog modules, currencies, and trading functionality. Some fields from this group may not be available on a specific site.

#|
|| **Field**
`type` | **Description** ||
|| **SETTINGS_IBLOCK_ID**
[`string`](../../data-types.md) \| [`integer`](../../data-types.md) | Identifier of the catalog information block ||
|| **SETTINGS_SECTION_ID**
[`string`](../../data-types.md) \| [`integer`](../../data-types.md) | Identifier of the catalog section ||
|| **SETTINGS_HIDE_NOT_AVAILABLE**
[`string`](../../data-types.md) | Mode for hiding unavailable products ||
|| **SETTINGS_HIDE_NOT_AVAILABLE_OFFERS**
[`string`](../../data-types.md) | Hide unavailable product variants: `Y` or `N` ||
|| **SETTINGS_PRODUCT_SUBSCRIPTION**
[`string`](../../data-types.md) | Enable product subscription: `Y` or `N` ||
|| **SETTINGS_USE_PRODUCT_QUANTITY**
[`string`](../../data-types.md) | Enable product stock accounting: `Y` or `N` ||
|| **SETTINGS_DISPLAY_COMPARE**
[`string`](../../data-types.md) | Show product comparison: `Y` or `N` ||
|| **SETTINGS_PRICE_CODE**
[`array`](../../data-types.md) | Array of price type codes ||
|| **SETTINGS_CURRENCY_ID**
[`string`](../../data-types.md) | Currency code ||
|| **SETTINGS_PRICE_VAT_INCLUDE**
[`string`](../../data-types.md) | Include VAT in price: `Y` or `N` ||
|| **SETTINGS_SHOW_OLD_PRICE**
[`string`](../../data-types.md) | Show old price: `Y` or `N` ||
|| **SETTINGS_SHOW_DISCOUNT_PERCENT**
[`string`](../../data-types.md) | Show discount percentage: `Y` or `N` ||
|| **SETTINGS_USE_PRICE_COUNT**
[`string`](../../data-types.md) | Enable price ranges: `Y` or `N` ||
|| **SETTINGS_SHOW_PRICE_COUNT**
[`integer`](../../data-types.md) | Number of prices to display ||
|| **SETTINGS_USE_ENHANCED_ECOMMERCE**
[`string`](../../data-types.md) | Enable enhanced e-commerce: `Y` or `N` ||
|| **SETTINGS_DATA_LAYER_NAME**
[`string`](../../data-types.md) | Name of the data layer for analytics ||
|| **SETTINGS_BRAND_PROPERTY**
[`string`](../../data-types.md) | Brand property code ||
|| **SETTINGS_CART_POSITION**
[`string`](../../data-types.md) | Position of the cart. Supported values are `TC`, `TR`, `CR`, `BR`, `BC`, `BL`, `CL`, `TL` ||
|| **SETTINGS_AGREEMENT_USE**
[`string`](../../data-types.md) | Enable agreements: `Y` or `N` ||
|| **SETTINGS_AGREEMENT_ID**
[`string`](../../data-types.md) \| [`integer`](../../data-types.md) | Identifier of the agreement ||
|| **SETTINGS_AGREEMENTS**
[`array`](../../data-types.md) | Array of agreements in the format `{ID, CHECKED, REQUIRED}` ||
|#

## Continue Learning

- [landing.landing.add](./methods/landing-landing-add.md)
- [landing.landing.update](./methods/landing-landing-update.md)
- [Page Color Themes](./color-themes.md)
- [landing.landing.getadditionalfields](./methods/landing-landing-get-additional-fields.md)
- [landing.syspage.set](./special-pages/landing-syspage-set.md)