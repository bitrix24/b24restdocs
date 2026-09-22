# Page Color Themes

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The `THEME_CODE` field sets a ready-made color palette for the page. The table lists the available codes and general color characteristics of the themes.

You can set the value of `THEME_CODE` when creating and updating a page through the methods [landing.landing.add](./methods/landing-landing-add.md) and [landing.landing.update](./methods/landing-landing-update.md).

You can retrieve the current value of `THEME_CODE` using the method [landing.landing.getadditionalfields](./methods/landing-landing-get-additional-fields.md) if the field is filled.

## Access Conditions

The methods require the [`landing`](../../scopes/permissions.md) scope. The required permissions depend on the action:

- `landing.landing.add` requires Edit permission for the site
- `landing.landing.update` requires Change settings permission for the site
- `landing.landing.getadditionalfields` requires View permission for the site

## How to Choose a Color

Choose how to configure the color:

- for a ready-made palette, pass one of the codes from the table in `THEME_CODE`
- for a custom color, pass `THEME_USE = Y` and `THEME_COLOR` in `#RRGGBB` format
- pass font settings separately in the `THEMEFONTS_*` fields

The color theme changes the color of buttons and some other elements. The exact set of elements depends on the block.

## How to Pass a Theme Code

For the `landing.landing.update` method, pass `THEME_CODE` in the `fields.ADDITIONAL_FIELDS` object:

```json
{
    "lid": 349,
    "fields": {
        "ADDITIONAL_FIELDS": {
            "THEME_CODE": "2business"
        }
    }
}
```

When creating a page, pass the same `fields.ADDITIONAL_FIELDS` object to the [landing.landing.add](./methods/landing-landing-add.md) method.

## Important Information

Blocks with a predefined color style may not change after you select a theme.

## Available Themes
#|
|| **Theme Code** | **Description** ||
|| `2business` | Purple-blue palette ||
|| `3corporate` | Blue palette ||
|| `app` | Turquoise palette ||
|| `accounting` | Yellow-green palette ||
|| `1construction` | Amber palette ||
|| `real-estate` | Orange-red palette ||
|| `photography` | Dark palette ||
|| `gym` | Rich blue palette ||
|| `wiki-dark` | Dark palette for wiki templates ||
|| `consulting` | Green-turquoise palette ||
|| `courses` | Aquamarine palette ||
|| `spa` | Citrus palette ||
|| `charity` | Yellow palette ||
|| `twentyFourth` | Golden-brown palette ||
|| `travel` | Vermilion palette ||
|| `architecture` | Sunset shades palette ||
|| `event` | Amaranth palette ||
|| `lawyer` | Carmine-pink palette ||
|| `restaurant` | Raspberry palette ||
|| `shipping` | Red palette ||
|| `agency` | Pastel-red palette ||
|| `music` | Bright pink-red palette ||
|| `wedding` | Cranberry palette ||
|| `twentyThird` | Purple palette ||
|#

## Continue Learning

- [{#T}](./additional-fields.md)
