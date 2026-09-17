# Navigation and Header

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Navigation and title is a scenario in which a block displays data of the current page: its title or its navigation chain. This is about two markers in the block markup, not about blocks with a site menu — those are covered in the [Menu Blocks](./menu.md) article.

The scenario suits section headers, covers, and breadcrumbs in an online store or a knowledge base: the same block is placed on different pages, and on each one it displays that page's own title. If the title in the block has to stay the same, or the chain of links has to be assembled manually, the markers are not used: the text is set through regular block nodes.

No dedicated `subtype` in the [block manifest](../manifest.md) is needed for this. It is sufficient to add one of the markers to the block's markup:

- `#title#` — the page title
- `#breadcrumb#` — the navigation chain of the page

The markers can be placed anywhere in the markup, and both can be used in the same block. They are retained in the block content as is, and the substitution happens when the page is rendered. That is why the read methods return different results:

- [landing.block.getContentFromRepository](../methods/landing-block-get-content-from-repository.md) returns the marker itself — this is the original template markup
- [landing.block.getcontent](../methods/landing-block-get-content.md) returns the substituted value: a placeholder title and a demo chain

## Marker `#title#`

In the public part, the marker `#title#` is replaced with the title of the current page. The title is taken from the page name — the `TITLE` field, which [landing.landing.getList](../../page/methods/landing-landing-get-list.md) returns and [landing.landing.update](../../page/methods/landing-landing-update.md) modifies. The set of page fields is described in the [Page Fields](../../page/fields.md) article.

Example of a block with a title:

```html
<section class="landing-block g-pt-20 g-pb-20">
    <div class="landing-title-container container g-font-size-12">
        #title#
    </div>
</section>
```

In the editor and the preview, a demo value is displayed instead of the real title.

## Marker `#breadcrumb#`

In the public part, the marker `#breadcrumb#` is replaced with the navigation chain of the current page. Bitrix24 builds the chain from the position of the page on the site: from the folder it is stored in and from the site section. The composition of the chain is determined by the page structure rather than by the block, so it cannot be influenced through the manifest or REST. To move a page to another folder, use [landing.landing.move](../../page/methods/landing-landing-move.md).

In the editor and the preview, a demo chain is substituted. The system takes its markup from the template of the main Bitrix24 site, not from the template of the site where the block is placed.

Example of a block with a navigation chain:

```html
<section class="landing-block g-pt-20 g-pb-20">
    <div class="landing-breadcrumb-container container g-font-size-12">
        #breadcrumb#
    </div>
</section>
```

## Examples of Standard Blocks

- `store.breadcrumb`
- `store.breadcrumb_dark_bg_text_left`
- `store.store_v3_menu_2`

## How to Reproduce This in Your Own Block

1. Review the markup of a ready-made block using the [landing.block.getContentFromRepository](../methods/landing-block-get-content-from-repository.md) method, passing a block code from the list above.
2. Place the required marker in the markup of your own block and register it using the [landing.repo.register](../../user-blocks/landing-repo-register.md) method.
3. Add the block to a page using the [landing.landing.addblock](../../page/block-methods/landing-landing-add-block.md) method and publish the page using the [landing.landing.publication](../../page/methods/landing-landing-publication.md) method. The result of the substitution is visible only on the published page.

A marker can also be added to a block that is already placed on a page: pass the new markup using the [landing.block.updatecontent](../methods/landing-block-update-content.md) method.

## Permissions and Limitations

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

Limitations:

- exactly two markers are replaced — `#title#` and `#breadcrumb#`. A custom marker cannot be added
- the marker works only within the block content. There is no dedicated parameter or method for substituting the title
- if the block has no marker, no substitution is performed and no extra text is added to the block

## Continue Your Learning

- [{#T}](./index.md)
- [{#T}](./menu.md)
- [{#T}](./maps.md)
- [{#T}](./search.md)
- [{#T}](./search-forms.md)
- [{#T}](./crm-forms.md)