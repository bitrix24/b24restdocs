# Additional Page Fields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Additional fields are page settings: the color theme and fonts, meta tags for search engines and social networks, analytics counters, custom code and styles. For example, they are used to connect Google Analytics to the page.

Set field values in the `fields.ADDITIONAL_FIELDS` object using the [landing.landing.update](./methods/landing-landing-update.md) method, and retrieve populated fields using the [landing.landing.getadditionalfields](./methods/landing-landing-get-additional-fields.md) method. For a new page, the fields are written the same way, right after it is created with the [landing.landing.add](./methods/landing-landing-add.md) method.

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can retrieve the fields: a user with the "view" access permission for the site. To change the fields, the "edit" and "edit settings" access permissions for the site are required

{% note info "" %}

To write the fields, pass the page identifier from the [landing.landing.add](./methods/landing-landing-add.md) or [landing.landing.getList](./methods/landing-landing-get-list.md) method in `lid`, and the field codes and values in `fields.ADDITIONAL_FIELDS`. Example request:

```json
{
    "lid": 349,
    "fields": {
        "ADDITIONAL_FIELDS": {
            "METAROBOTS_INDEX": "Y",
            "METAMAIN_USE": "Y",
            "METAMAIN_TITLE": "Page Title",
            "METAOG_IMAGE": 41003
        }
    }
}
```

{% endnote %}

## What You Need to Know

- Not all fields are available on a site: the set depends on the type of site, settings, and installed modules.
- System fields, such as `FONTS_CODE` and `SPEED_ASSETS`, may appear in the response of the [landing.landing.getadditionalfields](./methods/landing-landing-get-additional-fields.md) method, but you do not need to pass them to [landing.landing.update](./methods/landing-landing-update.md).
- For flag fields, use values `Y` and `N`.
- A `*_USE` flag enables its group of fields: for example, `METAMAIN_TITLE` appears on the page only if `METAMAIN_USE` equals `Y`.
- If a field is empty, the method [landing.landing.getadditionalfields](./methods/landing-landing-get-additional-fields.md) will not return it in the response.
- The fields `METAOG_IMAGE`, `BACKGROUND_PICTURE`, and `FAVICON_PICTURE` store an image: write the file identifier from the [landing.block.uploadfile](../block/methods/landing-block-upload-file.md) method to them, for example `41003`. You can also write a link to an image to `METAOG_IMAGE` and `BACKGROUND_PICTURE`.
- When reading an image field that contains a file identifier, an application receives a link to the file, and a webhook receives the identifier as a string, for example `"41003"`. A link to an image written to the field is returned as is to both the application and the webhook.
- The fields `SETTINGS_*` are catalog and store settings. If the catalog module is installed in Bitrix24, the [landing.landing.getadditionalfields](./methods/landing-landing-get-additional-fields.md) method returns them for pages of sites and stores with default values, even if they were not set. Fields with an empty default value, such as `SETTINGS_IBLOCK_ID`, are not included in the response. Knowledge Base pages have no `SETTINGS_*` fields.

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
[`string`](../../data-types.md) | Line height: from `0.7` to `2` in increments of `0.1` ||
|| **THEMEFONTS_FONT_WEIGHT**
[`string`](../../data-types.md) | Font weight of the main text: from `100` to `900` in increments of `100` ||
|| **THEMEFONTS_FONT_WEIGHT_H**
[`string`](../../data-types.md) | Font weight of headings: from `100` to `900` in increments of `100` ||
|| **FONTS_CODE**
[`string`](../../data-types.md) | System field with HTML for connecting fonts. May be present in the response of the reading method ||
|#

## Social Networks and SEO

#|
|| **Field**
`type` | **Description** ||
|| **METAOG_TITLE**
[`string`](../../data-types.md) | Title of the page preview in social networks and messengers, Open Graph tag `og:title` ||
|| **METAOG_DESCRIPTION**
[`string`](../../data-types.md) | Description of the page preview in social networks and messengers, Open Graph tag `og:description` ||
|| **METAOG_IMAGE**
[`string`](../../data-types.md) \| [`integer`](../../data-types.md) | Image for Open Graph, tag `og:image`. When writing, pass a file identifier or a link to an image. If the field contains a file identifier, an application receives a link to the file when reading the field, while a webhook receives the identifier itself ||
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
[`string`](../../data-types.md) | Enable custom `robots.txt`: `Y` or `N`. The field has no effect on a page: set it for the site with the [landing.site.update](../site/landing-site-update.md) method ||
|| **ROBOTS_CONTENT**
[`string`](../../data-types.md) | Content of the custom `robots.txt`. Like `ROBOTS_USE`, it works only for the site ||
|#

## Background, Animation, Icon, and Page View

#|
|| **Field**
`type` | **Description** ||
|| **BACKGROUND_USE**
[`string`](../../data-types.md) | Enable background settings for the page: `Y` or `N` ||
|| **BACKGROUND_PICTURE**
[`string`](../../data-types.md) \| [`integer`](../../data-types.md) | Background image of the page. When writing, pass a file identifier or a link to an image. If the field contains a file identifier, an application receives a link to the file when reading the field, while a webhook receives the identifier itself ||
|| **BACKGROUND_POSITION**
[`string`](../../data-types.md) | Background display mode. Supported values are `center`, `repeat`, `center_repeat_y`, `no_repeat` ||
|| **BACKGROUND_COLOR**
[`string`](../../data-types.md) | Background color of the page in HEX format ||
|| **FAVICON_PICTURE**
[`string`](../../data-types.md) \| [`integer`](../../data-types.md) | Favicon image: the page icon in the browser tab. When writing, pass a file identifier: if you pass a link to an image, the method stores it, but the icon does not appear on the page. If the field contains a file identifier, an application receives a link to the file when reading the field, while a webhook receives the identifier itself ||
|| **VIEW_USE**
[`string`](../../data-types.md) | Enable special page view: `Y` or `N` ||
|| **VIEW_TYPE**
[`string`](../../data-types.md) | Type of view: `no` — no margins, `ltr` — margins at the top and on the sides, `all` — margins on all sides, `mobile` — for mobile devices, `adaptive` — adaptive container ||
|| **PADDING_TOPBOTTOM**
[`string`](../../data-types.md) | Vertical outer margins of the page. Supported values are `N`, `u-outer-space-v1`, `u-outer-space-v2` ||
|| **PADDING_LEFTRIGHT**
[`string`](../../data-types.md) | Horizontal layout constraints. Supported values are `N`, `g-layout-semiboxed`, `g-layout-boxed` ||
|| **LAYOUT_BREAKPOINT**
[`string`](../../data-types.md) | Breakpoint for adaptive view. Supported values are `mobile`, `tablet`, `desktop`, `all` ||
|| **TRANSITION_COLOR**
[`string`](../../data-types.md) | Background color for the page appearance animation. The field has no effect on a page: set it for the site with the [landing.site.update](../site/landing-site-update.md) method ||
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
[`string`](../../data-types.md) | Enable Google Maps API: `Y` or `N`. The field has no effect on a page: set it for the site with the [landing.site.update](../site/landing-site-update.md) method ||
|| **GMAP_CODE**
[`string`](../../data-types.md) | Google Maps API key. Like `GMAP_USE`, it works only for the site ||
|#

## Bitrix24 Widget

#|
|| **Field**
`type` | **Description** ||
|| **B24BUTTON_USE**
[`string`](../../data-types.md) | Enable the Bitrix24 widget, a button on the page that visitors use to contact the company: `Y` or `N` ||
|| **B24BUTTON_CODE**
[`string`](../../data-types.md) | Link to the widget script that Bitrix24 loads on the page ||
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
[`string`](../../data-types.md) | Show cookies banner: `Y` or `N`. The `COOKIES_*` fields have no effect on a page: set them for the site with the [landing.site.update](../site/landing-site-update.md) method ||
|| **COOKIES_AGREEMENT_ID**
[`string`](../../data-types.md) \| [`integer`](../../data-types.md) | Identifier of the agreement whose text the cookies banner shows. The [userconsent.agreement.list](../../user-consent/user-consent-agreement-list.md) method returns the list of agreements ||
|| **COOKIES_COLOR_BG**
[`string`](../../data-types.md) | Background color of the cookies banner in HEX format ||
|| **COOKIES_COLOR_TEXT**
[`string`](../../data-types.md) | Text color of the cookies banner in HEX format ||
|| **COOKIES_POSITION**
[`string`](../../data-types.md) | Position of the cookies banner. Supported values are `bottom_left`, `bottom_right` ||
|| **COOKIES_MODE**
[`string`](../../data-types.md) | Mode of the cookies banner: `A` — visitor consent is required, `I` — only a notice about cookie collection ||
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
[`string`](../../data-types.md) | Use lazy load: `Y` or `N`. The field has no effect on a page: set it for the site with the [landing.site.update](../site/landing-site-update.md) method ||
|| **SPEED_USE_WEBPACK**
[`string`](../../data-types.md) | Use webpack resource build mode: `Y` or `N`. Like `SPEED_USE_LAZY`, it works only for the site ||
|#

## Catalog and Trading Section Settings

The set of fields in this group depends on which modules are installed: catalog, currencies, sales.

#|
|| **Field**
`type` | **Description** ||
|| **SETTINGS_IBLOCK_ID**
[`string`](../../data-types.md) \| [`integer`](../../data-types.md) | Identifier of the catalog information block ||
|| **SETTINGS_SECTION_ID**
[`string`](../../data-types.md) \| [`integer`](../../data-types.md) | Identifier of the catalog section ||
|| **SETTINGS_HIDE_NOT_AVAILABLE**
[`string`](../../data-types.md) | How to show unavailable products: `Y` — do not show, `L` — at the end of the list, `N` — in the general list ||
|| **SETTINGS_HIDE_NOT_AVAILABLE_OFFERS**
[`string`](../../data-types.md) | How to show unavailable product variants: `Y` — do not show, `L` — show only with the subscription option, `N` — show all ||
|| **SETTINGS_PRODUCT_SUBSCRIPTION**
[`string`](../../data-types.md) | Enable product subscription: `Y` or `N` ||
|| **SETTINGS_USE_PRODUCT_QUANTITY**
[`string`](../../data-types.md) | Allow the customer to specify the product quantity: `Y` or `N` ||
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
[`integer`](../../data-types.md) | Product quantity for which to select the price if the product has price ranges ||
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