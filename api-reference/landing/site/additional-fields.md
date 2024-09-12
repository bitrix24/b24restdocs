# Additional Fields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter mandatory status is not indicated
- links to pages that have not yet been created are not provided

{% endnote %}

{% endif %}

Fields are read through the method [landing.site.getadditionalfields](./landing-site-get-additional-fields.md).

{% note warning %}

Below are the field codes that must be specified in the array with the key ADDITIONAL for writing to the entity. For example, `ADDITIONAL_FIELDS: {UP_SHOW: 'Y'}`.

{% endnote %}

#|
|| **Field** | **Description** | **Read** | **Write** ||
|| **THEME_CODE**
[`unknown`](../../data-types.md) | Color palette. [Theme descriptions](../page/color-themes.md) | Yes | Yes ||
|| **THEME_CODE_TYPO**
[`unknown`](../../data-types.md) | Font settings. | Yes | Yes ||
|| **B24BUTTON_CODE**
[`unknown`](../../data-types.md) | Widget identifier on the site. The JS path to the widget is passed. For example, https://cdn.bitrix24.com/crm/loader_2_ibikwq.js | Yes | Yes ||
|| **B24BUTTON_COLOR**
[`unknown`](../../data-types.md) | Widget color, can take values: 
- site (use the main color of the site);
- button (use the color from the widget settings). | Yes | Yes ||
|| **UP_SHOW**
[`unknown`](../../data-types.md) | Show the Up button: Y/N. | Yes | Yes ||
|#

## Background Image

#|
|| **Field** | **Description** | **Read** | **Write** ||
|| **BACKGROUND_USE**
[`unknown`](../../data-types.md) | Use functionality: Y/N | Yes | Yes ||
|| **BACKGROUND_PICTURE**
[`unknown`](../../data-types.md) | Path to the image. | Yes | Yes ||
|| **BACKGROUND_POSITION**
[`unknown`](../../data-types.md) | Positioning: center (stretch), repeat (tile). | Yes | Yes ||
|| **BACKGROUND_COLOR**
[`unknown`](../../data-types.md) | Background color. | Yes | Yes ||
|#

## Analytics

#|
|| **Field** | **Description** | **Read** | **Write** ||
|| **YACOUNTER_USE**
[`unknown`](../../data-types.md) | Use Yandex.Metrica: Y/N. | Yes | Yes ||
|| **YACOUNTER_COUNTER**
[`unknown`](../../data-types.md) | Yandex.Metrica counter code. | Yes | Yes ||
|| **GACOUNTER_USE**
[`unknown`](../../data-types.md) | Use Google Analytics: Y/N. | Yes | Yes ||
|| **GACOUNTER_COUNTER**
[`unknown`](../../data-types.md) | Google Analytics counter code. | Yes | Yes ||
|| **GACOUNTER_SEND_CLICK**
[`unknown`](../../data-types.md) | Send click data for buttons and links to Google Analytics. | Yes | Yes ||
|| **GACOUNTER_SEND_SHOW**
[`unknown`](../../data-types.md) | Send data on page block views to Google Analytics. | Yes | Yes ||
|| **GTM_USE**
[`unknown`](../../data-types.md) | Use Google Tag Manager. | Yes | Yes ||
|| **GTM_COUNTER**
[`unknown`](../../data-types.md) | Google Tag Manager code. | Yes | Yes ||
|#

## Maps

#|
|| **Field** | **Description** | **Read** | **Write** ||
|| **GMAP_USE**
[`unknown`](../../data-types.md) | Use Google Maps: Y/N. | Yes | Yes ||
|| **GMAP_CODE**
[`unknown`](../../data-types.md) | Google Maps code. | Yes | Yes ||
|#

## Site View

#|
|| **Field** | **Description** | **Read** | **Write** ||
|| **VIEW_USE**
[`unknown`](../../data-types.md) | Use view: Y/N. | Yes | Yes ||
|| **VIEW_TYPE**
[`unknown`](../../data-types.md) | View type: 
- no (no view),
- ltr (margin on top and sides),
- all (margin on all sides). | Yes | Yes ||
|#

## Robots.txt

#|
|| **Field** | **Description** | **Read** | **Write** ||
|| **ROBOTS_USE**
[`unknown`](../../data-types.md) | Show your Robots.txt: Y/N. | Yes | Yes ||
|| **ROBOTS_CONTENT**
[`unknown`](../../data-types.md) | Content of robots.txt | Yes | Yes ||
|#

## Custom HTML

#|
|| **Field** | **Description** | **Read** | **Write** ||
|| **HEADBLOCK_USE**
[`unknown`](../../data-types.md) | Use: Y/N. | Yes | Yes ||
|| **HEADBLOCK_CODE**
[`unknown`](../../data-types.md) | HEAD block, arbitrary HTML. | Yes | Yes ||
|#

## Custom CSS

#|
|| **Field** | **Description** | **Read** | **Write** ||
|| **CSSBLOCK_USE**
[`unknown`](../../data-types.md) | Use: Y/N. | Yes | Yes ||
|| **CSSBLOCK_CODE**
[`unknown`](../../data-types.md) | Arbitrary CSS code. | Yes | Yes ||
|| **CSSBLOCK_FILE**
[`unknown`](../../data-types.md) | Link to CSS file. | Yes | Yes ||
|#