# Navigation and Header

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

For blocks of this type, there is no need to specify a special `subtype` in the manifest. It is sufficient to add one of the markers to the block's markup:

- `#title#`
- `#breadcrumb#`

The system replaces them during the processing of the block's content.

## Marker `#title#`

In the public part, the marker `#title#` is replaced with the title of the current page.

Example of a block with a title:

```html
<section class="landing-block g-pt-20 g-pb-20">
    <div class="landing-title-container container g-font-size-12">
        #title#
    </div>
</section>
```

In the preview and editor, a standard text value can be used.

## Marker `#breadcrumb#`

In the public part, the marker `#breadcrumb#` is replaced with the navigation chain of the current page.

In the public part, the chain is built using `getNavChain()`. In the preview and editor, the system may use the `chain_template.php` template of the active site template and substitute the template markup.

Example of a block with a navigation chain:

```html
<section class="landing-block g-pt-20 g-pb-20">
    <div class="landing-breadcrumb-container container g-font-size-12">
        #breadcrumb#
    </div>
</section>
```

## Important Considerations

- It is sufficient for the required marker to be present in the block's HTML.
- If these markers are not present in the block, special substitution will not be performed.

## Examples of Standard Blocks

- `store.breadcrumb`
- `store.breadcrumb_dark_bg_text_left`
- `store.store_v3_menu_2`

## Continue Your Learning

- [Special Blocks](./index.md)
- [Menu Blocks](./menu.md)
- [Maps in Blocks](./maps.md)
- [Search Results](./search.md)
- [Search Forms](./search-forms.md)
- [Forms in Blocks](./crm-forms.md)