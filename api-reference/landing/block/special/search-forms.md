# Search Forms

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed to meet writing standards

{% endnote %}

{% endif %}

Such blocks contain a form with a search input field and a button to submit data to the [search results page](./search.md).

The minimum requirements for the proper functioning of such a block are:

1. The presence of a `<form>` tag, an input field for the query `<input>`, and a submit button.
2. The presence of an attribute for the `<form>` selector.

    ```js
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
            'disableBlocks' => true
        ]
    ]
    ```
3. Optionally, you can add **subtype** and **subtype_params** to the block description. In this case, the attribute **Search result page** (see point 2) will be populated with the page when the block is added (which is convenient for the user):

    ```js
    'block' => [
        'name' => Loc::getMessage('LANDING_BLOCK_59_2-NAME'),
        'section' => array('sidebar', 'other'),
        'subtype' => 'search',
        'subtype_params' => [
            'type' => 'form',
            'resultPage' => 'result'
        ]
    ],
    ```

    Where **result** is the template code for the search results page. If such a page is found on the site, it will be added automatically.