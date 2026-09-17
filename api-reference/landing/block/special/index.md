# Special Blocks: Scenario Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Special blocks address specific scenarios rather than general layout tasks: page navigation, menus, search, maps, and embedding CRM forms. Such a block receives part of its content from Bitrix24: the system fills in the menu items, the link to the search results page, the CRM form code, or the initial map settings.

A scenario is enabled in the [block manifest](../manifest.md): specify `subtype` and, if needed, `subtype_params` in the `block` key. Navigation and the page title are the exception — markers in the block HTML are enough there.

This documentation covers the scenarios available for custom blocks. The `subtype` key has other values as well, but they serve the standard Bitrix24 blocks and cannot be configured via REST.

> Quick Navigation: [All scenarios section](#all-scenarios)
>
> User documentation: [Create and configure your Bitrix24 site](https://helpdesk.bitrix24.com/open/25743741/)

## Relationship with Other Objects

**Block Manifest.** Most of the special features are defined in the [manifest](../manifest.md): through the `block` section, the `assets` section, and attribute settings. The manifest file can be obtained using the [landing.block.getmanifestfile](../methods/landing-block-get-manifest-file.md) method.

**Block Methods.** Block content is read and modified by the methods of the [Working with Blocks](../methods/index.md) section.

**Interactive Blocks.** If the task is related not to system subtypes (`subtype`), but to the behavior of the block in the editor and on the client side, refer to the section [Interactive Blocks](../interactive/index.md).

## How to Choose a Scenario

#|
|| **Task** | **Scenario** | **What Enables It** ||
|| Fill a block with menu links automatically | [Menu Blocks](./menu.md) | `subtype: menu` ||
|| Embed an interactive map with markers | [Maps in Blocks](./maps.md) | `subtype: map` ||
|| Embed a CRM form and collect requests in CRM | [Forms in Blocks](./crm-forms.md) | `subtype: form` ||
|| Add a search form that submits a request to a separate page | [Search Forms](./search-forms.md) | `subtype: search` with `type: form` ||
|| Design a page that displays search results | [Search Results](./search.md) | Dynamic block cards, no subtype needed ||
|| Display the page title or the navigation chain | [Navigation and Title](./navigation.md) | Markers `#title#` and `#breadcrumb#` in the markup ||
|#

The search scenarios are used as a pair: [Search Forms](./search-forms.md) describes the block that submits the request, and [Search Results](./search.md) describes the page that processes it. [Navigation and Title](./navigation.md) usually complements another scenario, while menus, maps, and CRM forms more often address the primary task of the block.

## What the Manifest Keys Mean

- `subtype` — the scenario code in the `block` section of the manifest. The system uses it to find the handler that extends the block manifest: it describes nodes, adds attributes, and connects the required extensions. You can pass an array of subtypes: the handlers are applied in turn, but the `afterAdd` callback fires only for the last one
- `subtype_params` — the scenario parameters. For a search form, for example, this is the template code of the results page
- `afterAdd` — a callback that the handler registers when needed. It fires once, when the block is added to a page, and fills the block with Bitrix24 data

## How to Apply a Scenario

1. Review a ready-made implementation: retrieve the list of templates using the [landing.block.getrepository](../methods/landing-block-get-repository.md) method, and then the manifest of the required block using the [landing.block.getmanifestfile](../methods/landing-block-get-manifest-file.md) method. The codes of the standard blocks are listed on the scenario pages.
2. Reproduce the markup and the `subtype` key from the manifest in your own block.
3. Register the block using the [landing.repo.register](../../user-blocks/landing-repo-register.md) method, passing the manifest in the `manifest` parameter.
4. Add the block to a page using the [landing.landing.addblock](../../page/block-methods/landing-landing-add-block.md) method and check the result: the system performs the preparation when the block is added to the page.

## Permissions and Limitations

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

Limitations:

- the subtype is processed when the system builds the block manifest. A scenario cannot be assigned to a block already placed on a page via REST: the subtype is specified in the manifest at block registration
- the preparation relies on the markup. If the block lacks the required element, a map node or a form container for example, the subtype will not work
- some scenarios depend on the Bitrix24 context: the catalog menu depends on the online store, maps depend on the provider key, and CRM forms depend on the available forms

## Scenarios for Working with Special Blocks {#all-scenarios}

#|
|| **Documentation Section** | **Description** ||
|| [Menu Blocks](./menu.md) | Describes the setup of a block with auto-filled links from the catalog or account ||
|| [Maps in Blocks](./maps.md) | Shows the setup of a block with a map, auto-preparation of attributes, and provider behavior ||
|| [Navigation and Title](./navigation.md) | Explains the system substitution of markers `#title#` and `#breadcrumb#` in the block content ||
|| [Search Results](./search.md) | Describes the behavior of a page that accepts a request and displays the found pages of the site ||
|| [Search Forms](./search-forms.md) | Shows the setup of a search form and auto-insertion of a link to the results page ||
|| [Forms in Blocks](./crm-forms.md) | Describes the embedding of a CRM form, editor settings, and system substitution of the form code ||
|#
