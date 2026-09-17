# Interactive Blocks: Scenario Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Interactive blocks add client-side behavior to a page without separate script development: a slider, a gallery, or a countdown timer. A ready-made extension is connected in the [block manifest](../manifest.md), the elements are marked up with service classes, and the behavior is configured with `data-*` attributes. Bitrix24 initializes the rest itself when the page is rendered.

There are three ready-made scenarios. Behavior that is not among them is implemented with a custom script, which is connected in the `assets` key of the manifest.

> Quick Navigation: [All Scenarios Section](#all-scenarios)
>
> User documentation: [Create and configure your Bitrix24 site](https://helpdesk.bitrix24.com/open/25743741/)

## Relationship with Other Objects

**Block Manifest.** The scenario is determined by the set of extensions in the `assets.ext` key, while the behavior settings are described in the `attrs` key. The structure of the file is covered in the [Block Manifest](../manifest.md) article.

**Block Methods.** The content and attributes of a block placed on a page are modified by the methods of the [Working with Blocks](../methods/index.md) section: [landing.block.updateattrs](../methods/landing-block-update-attrs.md) for `data-*` settings and [landing.block.updatenodes](../methods/landing-block-update-nodes.md) for the content of slides and cards.

**Special Blocks.** If the task relates to menus, navigation, search, maps, or CRM forms, refer to the [Special Blocks](../special/index.md) section.

## How to Choose a Scenario

#|
|| **If You Need To** | **Use the Scenario** ||
|| Display the images of a block in a viewer | [Galleries](./gallery.md) ||
|| Scroll through the content of a block in place, without reloading the page | [Sliders](./sliders.md) ||
|| Display the time remaining until a given date | [Countdown Timers](./timer.md) ||
|#

What exactly each scenario covers is listed in the [Scenarios for Working with Interactive Blocks](#all-scenarios) table.

## How to Apply a Scenario

1. Review a ready-made implementation: retrieve the list of repository blocks using the [landing.block.getrepository](../methods/landing-block-get-repository.md) method, and then the manifest of the required block using the [landing.block.getmanifestfile](../methods/landing-block-get-manifest-file.md) method. The codes of the standard blocks are listed on the scenario pages, in the "Examples of Standard Blocks" section.
2. Connect the extension in `assets.ext` of your manifest and mark up the elements with service classes. Each scenario has its own extension and its own classes: `landing_gallery_cards` and `.js-gallery-cards`, `landing_carousel` and `.js-carousel`, `landing_countdown` and `.js-countdown`. Example for a slider:

   ```php
   'assets' => [
       'ext' => ['landing_carousel'],
   ],
   ```

   ```html
   <div class="js-carousel" data-slides-show="1">
       <div class="js-slide">Slide 1</div>
   </div>
   ```

3. Configure the behavior through `data-*` attributes. For a slider and a timer they are set on the container, for a gallery — on the images themselves. Describe the attributes the user is expected to change in the editor in the [attrs](../attributes.md) key of the manifest.
4. Register the block using the [landing.repo.register](../../user-blocks/landing-repo-register.md) method and add it to a page using the [landing.landing.addblock](../../page/block-methods/landing-landing-add-block.md) method.

There is no need to list the extension dependencies: Bitrix24 connects them itself. Together with `landing_gallery_cards`, the extensions `landing_core` and `landing_fancybox` are connected. The first one handles the basic initialization of block extensions, the second one opens the viewer. Together with `landing_carousel` and `landing_countdown`, the extensions `landing_core` and `landing_jquery` are connected: the slider and the timer are built on jQuery libraries.

## Permissions and Limitations

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

Limitations:

- the behavior is enabled by the connected extension rather than by a dedicated `subtype`. Without the extension in `assets.ext`, the markup stays static
- the extensions find elements by service CSS classes: `.js-carousel` for the slider, `.js-gallery-cards` for the gallery, `.js-countdown` for the timer. A custom container class cannot be set
- the values of `data-*` attributes are modified by the [landing.block.updateattrs](../methods/landing-block-update-attrs.md) method, but only for the attributes described in the block manifest
- in edit mode, part of the behavior is forcibly disabled: the gallery does not open and slider looping is turned off. Check the result in the preview or on the published page
- when the slider and the gallery are used together, connect `landing_carousel` first and `landing_gallery_cards` second

## Scenarios for Working with Interactive Blocks {#all-scenarios}

#|
|| **Documentation Section** | **Description** ||
|| [Galleries](./gallery.md) | Describes connecting `landing_gallery_cards`, markup structure, and image behavior in the viewer ||
|| [Sliders](./sliders.md) | Shows connecting `landing_carousel`, key `data-*` parameters, and behavior features in the editor ||
|| [Countdown Timers](./timer.md) | Describes connecting `landing_countdown`, the format of `data-end-date`, and the markup of the timer ||
|#
