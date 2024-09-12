# Additional Fields of the Entity Page

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter mandatory status not specified

{% endnote %}

{% endif %}

Identical fields with the Site have a higher priority for the page.

Fields can be read using the method [landing.landing.getadditionalfields](./methods/landing-landing-get-additional-fields.md).

{% note warning %}

Below are the field codes that must be specified in the array with the key ADDITIONAL for writing to the entity. For example, `ADDITIONAL_FIELDS => [METAROBOTS_INDEX => Y]`.

{% endnote %}

## Themes

#|
|| **Field** | **Description** | **Read** | **Write** ||
|| **THEME_CODE**
[`unknown`](../../data-types.md) | Color palette. [Theme descriptions](./color-themes.md) | Yes | Yes ||
|| **THEME_CODE_TYPO**
[`unknown`](../../data-types.md) | Font settings. | Yes | Yes ||
|#

## Social Media Preview

#|
|| **METAOG_TITLE**
[`unknown`](../../data-types.md) | Title, tag `og:title`. | Yes | Yes ||
|| **METAOG_DESCRIPTION**
[`unknown`](../../data-types.md) | Description, tag `og:description`. | Yes | Yes ||
|| **METAOG_IMAGE**
[`unknown`](../../data-types.md) | Image, tag `og:image`. | Yes | Yes ||
|#

## Meta Tags

#|
|| **METAMAIN_USE**
[`unknown`](../../data-types.md) | Set meta tags: Y/N. | Yes | Yes ||
|| **METAMAIN_TITLE**
[`unknown`](../../data-types.md) | Title, tag `title`. | Yes | Yes ||
|| **METAMAIN_DESCRIPTION**
[`unknown`](../../data-types.md) | Description, tag `description`. | Yes | Yes ||
|| **METAMAIN_KEYWORDS**
[`unknown`](../../data-types.md) | Keywords, tag `keywords`. | Yes | Yes ||
|#

## Background Image

#|
|| **BACKGROUND_USE**
[`unknown`](../../data-types.md) | Use functionality: Y/N | Yes | Yes ||
|| **BACKGROUND_PICTURE**
[`unknown`](../../data-types.md) | Path to the image. | Yes | Yes ||
|| **BACKGROUND_POSITION**
[`unknown`](../../data-types.md) | Positioning:
- center (stretch),
- repeat (tile). | Yes | Yes ||
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
[`unknown`](../../data-types.md) | Send data about page block views to Google Analytics. | Yes | Yes ||
|| **GTM_USE**
[`unknown`](../../data-types.md) | Use Google Tag Manager. | Yes | Yes ||
|| **GTM_COUNTER**
[`unknown`](../../data-types.md) | Google Tag Manager code. | Yes | Yes ||
|#

## Custom HTML

#|
|| **HEADBLOCK_USE**
[`unknown`](../../data-types.md) | Use: Y/N. | Yes | Yes ||
|| **HEADBLOCK_CODE**
[`unknown`](../../data-types.md) | HEAD block, arbitrary HTML. | Yes | Yes ||
|#

## Custom CSS

#|
|| **CSSBLOCK_USE**
[`unknown`](../../data-types.md) | Use: Y/N. | Yes | Yes ||
|| **CSSBLOCK_CODE**
[`unknown`](../../data-types.md) | Arbitrary CSS code. | Yes | Yes ||
|| **CSSBLOCK_FILE**
[`unknown`](../../data-types.md) | Link to CSS file. | Yes | Yes ||
|#

## Page View

#|
|| **VIEW_USE**
[`unknown`](../../data-types.md) | Use view: Y/N. | Yes | Yes ||
|| **VIEW_TYPE**
[`unknown`](../../data-types.md) | View type:
- no (no view),
- ltr (top and side margins),
- all (margins on all sides). | Yes | Yes ||
|#

## Other

#|
|| **METAROBOTS_INDEX**
[`unknown`](../../data-types.md) | Index the page in search engines: Y/N. | Yes | Yes ||
|#