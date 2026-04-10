# Interactive Blocks: Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Interactive blocks in `landing` add client-side behavior without the need for separate script development. For these blocks, you connect the required extension in the [block manifest](../manifest.md), configure the markup and attributes, after which the platform automatically initializes the behavior.

> Quick Navigation: [All Scenarios Section](#all-scenarios)

## Related Scenarios

**Block Manifest.** The type of interactive scenario is determined by the set of connected extensions in `assets.ext`. The manifest can be obtained via [landing.block.getmanifestfile](../methods/landing-block-get-manifest-file.md).

**Block Methods.** To analyze ready-made templates, use [landing.block.getrepository](../methods/landing-block-get-repository.md), then open the manifest of the selected block through [landing.block.getmanifestfile](../methods/landing-block-get-manifest-file.md).

**Special Blocks.** If the task relates to menus, navigation, search, maps, or CRM forms, refer to the [Special Blocks](../special/index.md) section.

## Choosing a Scenario

Open the subsection if you need to:

- connect a slider based on `js-carousel` and configure behavior through `data-*` attributes — [Sliders](./sliders.md),
- connect image viewing in a gallery via `js-gallery-cards` and `data-fancybox` — [Galleries](./gallery.md),
- add a countdown timer with an end date in `data-end-date` — [Countdown Timers](./timer.md).

## Considerations for Configuration

- The `landing_gallery_cards` extension connects with dependencies `landing_core` and `landing_fancybox`, while `landing_carousel` and `landing_countdown` connect with `landing_jquery`.
- `landing_gallery_cards` operates on the CSS selector `.js-gallery-cards` and elements with the HTML attribute `data-fancybox`, while in edit mode, the gallery is forcibly disabled.
- `landing_carousel` works on the CSS selector `.js-carousel`, and in edit mode, looping is forcibly disabled to avoid creating slide clones.
- `landing_countdown` initializes nodes based on the CSS selector `.js-countdown`, responds to changes in the HTML attribute `data-end-date`, and reinitializes after changes to the cards.
- When using both the slider and the gallery, first connect `landing_carousel`, then `landing_gallery_cards`.

## Scenarios for Working with Interactive Blocks {#all-scenarios}

#|
|| **Documentation Section** | **Description** ||
|| [Galleries](./gallery.md) | Describes connecting `landing_gallery_cards`, markup structure, and image behavior in the viewer ||
|| [Sliders](./sliders.md) | Shows connecting `landing_carousel`, key `data-*` parameters, and behavior features in the editor ||
|| [Countdown Timers](./timer.md) | Describes connecting `landing_countdown`, the format of `data-end-date`, and the markup of the timer ||
|#