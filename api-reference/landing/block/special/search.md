# Search Results

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A search results page is a regular site page that accepts a search query and displays the pages found. Site search consists of two parts: the [search form](./search-forms.md) block, which submits the query, and the results page, which processes it.

The scenario is meant for sites with a large number of pages. The standard search form blocks are designed for knowledge bases — sites of the `knowledge` and `group` types. On a regular site or in an online store, the form block is built manually: the markup and the subtype are taken from a standard block.

The results page has no subtype of its own. It is assembled from regular blocks with dynamic cards enabled, while the `search` subtype is specified in the form block rather than here.

## How the Query Reaches the Results Page

A search form is a `<form>` tag with a text field named `q`. The `action` attribute of the form holds the link to the results page, so on submit the browser opens that page and passes the query in the `q` parameter:

`https://example.bitrix24.site/search-result/?q=service agreement`

Such a link can also be opened directly, without the form: the results page reads the `q` parameter from the address. The browser encodes the query value, so a space in the address bar appears as `%20`: `?q=service%20agreement`.

The page itself does not search for anything. The search is performed by a block with dynamic cards: it reads the query and fills the cards with the matching pages of the site. That is why the page must contain at least one such block.

## What Dynamic Cards Are

Dynamic cards are a block mode in which the cards are filled not manually, but with data from a selected source. The mode is enabled in the block settings on the results page, and the source is selected there as well.

Search requires the source that is called `Site Pages` in the interface and recorded with the `landing:landing` code in the block parameters. The source fills the cards with the pages of the current site that match the query, while the settings define the sort order and the number of pages in the output.

For a block to support this mode, its manifest must describe an attribute of the `dynamic_source` type. The fields of such an attribute are listed in the [Attributes](../attributes.md#attribute-fields) article.

REST has no dedicated method for configuring dynamic cards: the mode and the source are set in the Bitrix24 editor. The current source parameters of a block are returned by [landing.block.getcontent](../methods/landing-block-get-content.md) in the `dynamicParams` field.

## How to Set Up Site Search

1. Add a search form block to the page, the standard `59.1.search` block for example. Form setup is described in the [Search Forms](./search-forms.md) article.
2. Get the results page:

   - if `subtype_params.resultPage` is set in the form block manifest, the system finds or creates the page itself when the block is added and records the link to it in the `action` attribute of the form
   - if `resultPage` is not set, create the page in advance using the [landing.landing.add](../../page/methods/landing-landing-add.md) method and specify it in the form settings manually

3. Configure the output on the results page: enable dynamic cards for the block and select the `Site Pages` source. Without this, the page opens, but no pages found appear on it.
4. Publish both pages using the [landing.landing.publication](../../page/methods/landing-landing-publication.md) method: a draft is not available to visitors.

You can check the result at the address of the results page with the `q` parameter: on the published page, the pages found appear in the cards of the block.

## Standard Templates of the Results Page

The system creates the results page from a template. There are three standard templates, and each one is already assembled from a form block, a heading, and a block with dynamic cards:

#|
|| **Template** | **What It Contains** | **Which Form Block It Holds** ||
|| `search-result` | Light version: a full-width form on a background image, with the list of results below | `59.1.search` ||
|| `search-result2` | Light version with the form in a sidebar | `59.2.search_sidebar` ||
|| `search-result3-dark` | Dark version with a section heading above the form | `59.3.search_dark` ||
|#

A template is connected by the code from the first column: specify it in `subtype_params.resultPage` of your own form block manifest. To view the manifest of a standard block and find out which template is set in it, use the [landing.block.getmanifestfile](../methods/landing-block-get-manifest-file.md) method, passing the block code.

## Permissions and Limitations

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

Limitations:

- the search looks through the pages of the current site. Other Bitrix24 objects are not found through this scenario
- the system creates the results page from a template only when the form block is added. For blocks already placed on a page, the substitution is not repeated
- when a site configuration is transferred between Bitrix24 accounts, the results page is not created: the form manifest preparation is not performed in this mode

## Continue Your Exploration

- [{#T}](./index.md)
- [{#T}](./menu.md)
- [{#T}](./maps.md)
- [{#T}](./navigation.md)
- [{#T}](./search-forms.md)
- [{#T}](./crm-forms.md)