# Search Forms

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Search forms are blocks that contain an input field and a submit button. They send a request to the [search results page](./search.md).

The scenario is meant for sites with many pages, where searching is easier for a visitor than browsing the menu. The form block is placed in the header, in a sidebar, or in a dedicated section of a knowledge base or an online store. If there are few pages, search is replaced with a menu or navigation.

Such blocks are described with the `search` subtype and the `type: form` parameter in the [block manifest](../manifest.md). The subtype does one thing: it fills the form with the link to the results page.

## Requirements for the Block to Function

The minimum requirements are:

- `<form>` tag
- Input field for the search query
- Submit button for the form
- `action` attribute configured through the [attrs](../attributes.md) key of the manifest

Example of the form attribute description:

```php
'attrs' => [
    '.landing-block-node-form' => [
        'name' => 'Search result page',
        'attribute' => 'action',
        'type' => 'url',
        'allowedTypes' => [
            'landing',
        ],
        'disableCustomURL' => true,
        'disallowType' => true,
        'disableBlocks' => true,
    ],
],
```

In standard blocks, the `action` attribute is set for the selector `.landing-block-node-form`. The keys of the example:

- `type: url` — a link selection field in the editor
- `attribute: action` — the DOM attribute the link is retained in
- `allowedTypes: ['landing']` — restricts the choice to site pages
- `disableCustomURL`, `disallowType`, `disableBlocks` — prohibit entering an address manually, changing the link type, and selecting a block

The common attribute fields and their types are covered in the [Attributes](../attributes.md#attribute-fields) article.

## How `subtype: search` Works

Example of the subtype description:

```php
'block' => [
    'subtype' => 'search',
    'subtype_params' => [
        'type' => 'form',
        'resultPage' => 'search-result',
    ],
],
```

After adding the block, the system:

- Searches the current site for a page with the template `search-result`
- If the page is found, it substitutes it into `action`
- If the page is not found, it creates it based on the template and then substitutes it into `action`

Subtype parameters:

#|
|| **Parameter** | **What It Defines** ||
|| `type` | The role of the block in the search scenario. Only one value is supported — `form`. With any other value, or without the parameter, the subtype does not modify the manifest ||
|| `resultPage` | The template code of the results page. The system uses it to look for a page on the current site in the `TPL_CODE` field, and if none is found, creates a page from that template ||
|#

To substitute the link, the system looks in the manifest for an attribute with `type: url` and `attribute: action`. The value found is recorded in the `#landing<ID>` format, where `ID` is the identifier of the results page.

## Examples of Standard Blocks

- `59.1.search`
- `59.2.search_sidebar`
- `59.3.search_dark`

## How to Build Your Own Form Block

1. Review the manifest of a standard block using the [landing.block.getmanifestfile](../methods/landing-block-get-manifest-file.md) method, passing a block code from the list above.
2. Describe `subtype: search` with the `type: form` and `resultPage` parameters in your own manifest, together with the `action` attribute for the form selector.
3. Register the block using the [landing.repo.register](../../user-blocks/landing-repo-register.md) method, passing the manifest in the `manifest` parameter.
4. Add the block to a page using the [landing.landing.addblock](../../page/block-methods/landing-landing-add-block.md) method. The system substitutes the link to the results page at this step.
5. Check the attribute value using the [landing.block.getcontent](../methods/landing-block-get-content.md) method with the `editMode = true` parameter — without it, the published version of the block is returned. To point to a different page, record it using the [landing.block.updateattrs](../methods/landing-block-update-attrs.md) method.

## Permissions and Limitations

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

Limitations:

- the subtype fires once, when the block is added to a page. For blocks already placed on a page, the link in `action` is not updated
- only a page of the current site can be substituted into `action`: an arbitrary URL and block binding are disabled for this attribute
- if `resultPage` is not specified, the results page is not substituted. It is selected manually in the block settings or recorded using the [landing.block.updateattrs](../methods/landing-block-update-attrs.md) method
- when a site configuration is transferred between Bitrix24 accounts, the manifest preparation is not performed, so the results page is not created
- the standard form blocks are intended for sites of the `knowledge` and `group` types
- the subtype is responsible only for the link in the form. What to configure on the results page itself is described in the [Search Results](./search.md) article

## Continue Your Learning

- [{#T}](./index.md)
- [{#T}](./menu.md)
- [{#T}](./maps.md)
- [{#T}](./navigation.md)
- [{#T}](./search.md)
- [{#T}](./crm-forms.md)