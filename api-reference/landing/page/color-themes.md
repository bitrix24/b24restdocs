# Page Color Themes

This section lists the color theme codes for the `THEME_CODE` field.

You can set the value of `THEME_CODE` when creating and updating a page through the methods [landing.landing.add](./methods/landing-landing-add.md) and [landing.landing.update](./methods/landing-landing-update.md).

You can retrieve the current value of `THEME_CODE` using the method [landing.landing.getadditionalfields](./methods/landing-landing-get-additional-fields.md) if the field is filled.

## Important Information

- `THEME_CODE` defines the ready-made color palette for the page.
- Font settings are stored separately in the `THEMEFONTS_*` fields.
- If you need to set a custom color, use `THEME_USE = Y` and `THEME_COLOR = #RRGGBB`.
- When migrating old typo settings, the module maps some color themes and font sets by theme code.

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
|| `twentyFourth` | Special palette with code `twentyFourth` ||
|| `travel` | Vermilion palette ||
|| `architecture` | Sunset shades palette ||
|| `event` | Amaranth palette ||
|| `lawyer` | Carmine-pink palette ||
|| `restaurant` | Raspberry palette ||
|| `shipping` | Red palette ||
|| `agency` | Pastel-red palette ||
|| `music` | Bright pink-red palette ||
|| `wedding` | Cranberry palette ||
|| `twentyThird` | Special palette with code `twentyThird` ||
|#

For a detailed description of all related fields, see the section [Additional Page Fields](./additional-fields.md).