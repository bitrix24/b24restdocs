# Search Forms

Search forms are blocks that contain an input field and a submit button. They send a request to the [search results page](./search.md).

For such blocks in `landing`, `subtype: search` is used with the parameter `type: form`.

## Requirements for the Block to Function

The minimum requirements are:

- `<form>` tag
- Input field for the search query
- Submit button for the form
- `action` attribute configured via `attrs` for the form

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

In standard blocks, the `action` attribute is set for the selector `.landing-block-node-form`.

## How `subtype: search` Works

If the manifest specifies:

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

`resultPage` is the code for the search results page template. The substitution occurs in `afterAdd`, immediately after the block is added.

## Important Considerations

- The subtype works when `subtype_params` specifies `type: form`
- The system searches the manifest for an attribute with `type: url` and `attribute: action`
- In standard blocks, the selector `.landing-block-node-form` is used for this purpose
- If `resultPage` is not specified, the subtype will not automatically substitute the results page
- The results page is searched on the current site by the template code `TPL_CODE`
- When transferring configuration via `AppConfiguration::inProcess()`, the preparation of the manifest is not performed

## Examples of Standard Blocks

- `59.1.search`
- `59.2.search_sidebar`
- `59.3.search_dark`

To ensure the search form functions correctly, configure the `action` attribute and specify the results page template in `resultPage`.

## Continue Your Learning

- [Special Blocks](./index.md)
- [Menu Blocks](./menu.md)
- [Maps in Blocks](./maps.md)
- [Navigation and Header](./navigation.md)
- [Search Results](./search.md)
- [Forms in Blocks](./crm-forms.md)