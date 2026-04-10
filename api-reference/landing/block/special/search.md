# Search Results

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The search results page works in conjunction with the [Search Forms](./search-forms.md) blocks. The search form sends a request to a separate page on the site, and this page displays the found pages through dynamic cards.

## Requirements for the Search Results Page

To ensure the search results page functions correctly, configure two elements:

- a search form block with `subtype: search` and `subtype_params.type: form`
- a results page where dynamic cards are enabled and the source `Site Pages` is selected

For standard form blocks, the system automatically populates the results page in the `action` attribute of the form. To achieve this, the manifest of the block must describe the URL attribute with `attribute: action`. In standard blocks, the link is recorded in the format `#landing<ID>`. The `action` attribute also has restrictions: only a site page can be selected, without arbitrary URLs or block bindings.

## How the System Populates the Results Page

The search subtype works only for blocks that specify `type: form` in `subtype_params`.

After adding such a block to the page, the system:

- retrieves the template code for the results page from `subtype_params.resultPage`
- searches for the results page on the current site using the template code
- if the page is found, it uses it
- if the page is not found, it creates it based on the template
- records the link to the found or created page in the `action` of the form

The substitution occurs in the `afterAdd` callback. During the transfer via `AppConfiguration::inProcess()`, this preparation does not initiate.

## Important Considerations

- auto-substitution works only for search forms, not for the results page
- if `resultPage` is not present in `subtype_params`, the system will not automatically populate the results page
- if dynamic cards are not enabled on the results page and the source `Site Pages` is not selected, the search will not display site pages

## Standard Templates and Blocks

The source code includes standard templates for results pages:

- `search-result`
- `search-result2`
- `search-result3-dark`

Standard search form blocks that use these templates:

- `59.1.search` → `search-result`
- `59.2.search_sidebar` → `search-result`
- `59.3.search_dark` → `search-result3-dark`

These blocks are intended for sites of types `knowledge` and `group`. The blocks `59.2.search_sidebar` and `59.3.search_dark` additionally pertain to sidebar variants.

The template `search-result2` is included in the standard templates for results pages, but it is not used in this set of standard search forms.

## Continue Your Exploration

- [Special Blocks](./index.md)
- [Menu Blocks](./menu.md)
- [Maps in Blocks](./maps.md)
- [Navigation and Header](./navigation.md)
- [Search Forms](./search-forms.md)
- [Forms in Blocks](./crm-forms.md)