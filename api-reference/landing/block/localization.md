# Localization of the Block

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet the writing standard

{% endnote %}

{% endif %}

Localization of blocks is possible starting from version 18.5.6. Translation can be done into any number of languages. To localize:

1. Create a block for your native language.
2. In the manifest, add two keys:

   ```js
    'lang' => [
        'en' => [
            'Title with a separator on a light background' => 'Title with a separator on a light background (translated)'
        ],
        'de' => [
            'Title with a separator on a light background' => 'Überschrift mit einem Trennzeichen auf einem hellen Hintergrund'
        ]
    ],
    'lang_original' => 'ru'
    ```

    Where:
    - **lang_original** – the language in which the block manifest is created. Note: specifically the phrases in the manifest.
    - **lang** – all other languages, at least one, otherwise what is the point of localization. (No one prohibits creating blocks only for French and Russian, for example.)

Let's take a closer look at the **lang** array. Its keys list all possible fields name in your manifest. This includes the name of the block itself, as well as the names of nodes, styles, attributes, and their values.

The system traverses the entire manifest, and if it encounters a key **name**, it looks for a suitable replacement in the language array (either the current language or en, if the account language differs from the block's native language) and replaces it.