# Special Blocks: Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Special blocks in `landing` address specific scenarios rather than general layout tasks: page navigation, menus, search, maps, and embedding CRM forms. The behavior of these blocks depends on the context of the site, the page, and the available objects.

> Quick Navigation: [All scenarios section](#all-scenarios)

## Related Scenarios

**Block Manifest.** Most of the special features are defined in the [manifest](../manifest.md): through the `block` section, the `assets` section, and attribute settings. The manifest file can be obtained using the [landing.block.getmanifestfile](../methods/landing-block-get-manifest-file.md) method.

**Block Methods.** The section [Methods for Working with Blocks](../methods/index.md) describes the tools for reading and modifying the content of a block. To explore ready-made implementations, first retrieve the list of blocks using [landing.block.getrepository](../methods/landing-block-get-repository.md), then open the manifest of the desired block using [landing.block.getmanifestfile](../methods/landing-block-get-manifest-file.md).

**Interactive Blocks.** If the task is related not to system subtypes (`subtype`), but to the behavior of the block in the editor and on the client side, refer to the section [Interactive Blocks](../interactive/index.md).

## Choosing a Scenario

Open the subsection if you need to:

- display the page title or navigation chain using system markers — [Navigation and Title](./navigation.md),

- automatically populate the block with menu links — [Menu Blocks](./menu.md),

- add a search form that submits a request to a separate page — [Search Forms](./search-forms.md),

- format a page that displays search results — [Search Results](./search.md),

- embed a map and prepare the block to work with the map provider — [Maps in Blocks](./maps.md),

- embed a CRM form in the block — [Forms in Blocks](./crm-forms.md).

## Considerations When Choosing a Scenario

- `search-forms` and `search` are usually used together: the first page describes the search form, while the second describes the results page.

- `navigation` typically complements other scenarios, while `maps`, `crm-forms`, and `menu` more often address the primary task of the block.

- Scenarios for [Menu Blocks](./menu.md), [Maps in Blocks](./maps.md), [Search Forms](./search-forms.md), and [Forms in Blocks](./crm-forms.md) are defined through `subtype` in the manifest, while markers in HTML are used for [Navigation and Title](./navigation.md).

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