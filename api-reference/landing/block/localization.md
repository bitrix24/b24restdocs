# Localization of the Block

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Block localization defines translations for the interface labels a user sees in the site editor: the block name, and the names of nodes, cards, attribute fields, and menu items. Translations are described in the [manifest file](./manifest.md) through the `lang_original` and `lang` keys.

Localization is intended for custom blocks that an application adds to the repository using the [landing.repo.register](../user-blocks/landing-repo-register.md) method. A typical case is a Market application with its own blocks, installed in Bitrix24 accounts with different interface languages. System blocks of Bitrix24 are translated through the product language files and do not use the `lang_original` and `lang` keys.

Two clarifications about the scope:

- localization does not translate block content. The text, links, and images a user has entered in a block on a page are stored in the content and remain in the original language
- the `lang_original` and `lang` keys with the same names also exist for custom site templates, but that is a separate mechanism with a different set of translatable fields. It is described in the [Template Localization](../demos/localization.md) article

## How to Add Localization

1. Build the block manifest in one language.
2. Pass the code of that language in `lang_original`.
3. Build the `lang` array: for each language, list the original manifest phrases and their translations.
4. Register the block using the [landing.repo.register](../user-blocks/landing-repo-register.md) method, passing `lang_original` and `lang` in the `manifest` parameter.
5. Add the block to a page and call [landing.block.getmanifest](./methods/landing-block-get-manifest.md). In the response, the `name` values must arrive in the Bitrix24 language, the `lang` key must be absent, and `lang_original` must be retained as it was.

## Localization Keys in the Manifest

- `lang_original` — the original language of the phrases in the manifest, for example `en`
- `lang` — the set of translations by language

The language code matches the Bitrix24 language code. The current language is returned by the [app.info](../../common/system/app-info.md) method in the `LANGUAGE_ID` field.

In the `lang` array, the keys are the original phrases from the manifest, and the values are the translations of those phrases.

Example:

```php
'lang_original' => 'en',
'lang' => [
    'de' => [
        'Title with a separator on a light background' => 'Titel mit Trennlinie auf hellem Hintergrund',
        'Button' => 'Schaltflache',
    ],
    'fr' => [
        'Title with a separator on a light background' => 'Titre avec separateur sur fond clair',
        'Button' => 'Bouton',
    ],
],
```

## Which Labels Are Translated {#translatable-keys}

The system iterates through the entire manifest and substitutes values only for `name` keys at any nesting level. Other keys are not translated, even if `lang` contains a matching phrase.

The `name` keys that occur in a manifest:

#|
|| **Manifest Key** | **What This Label Is** ||
|| `block.name` | The block name in the block catalog and in the repository list ||
|| `nodes.<selector>.name` | The [node](./node-types.md) name in the block editing form ||
|| `cards.<selector>.name` | The name of a [card](./extended-description.md) group ||
|| `cards.<selector>.presets.<code>.name` | The card preset name in the list ||
|| `style.nodes.<selector>.name` | The element label in the style panel, if it is set through the `name` key ||
|| `attrs.<selector>[].name` | The [attribute](./attributes.md) field name in the editor. For a group element — the name of the group itself ||
|| `attrs.<selector>[].attrs[].name` | The name of a field inside an attribute group ||
|| `attrs.<selector>[].items[].name` | The option label in list attribute types: `dropdown`, `checkbox`, `radio`, `multiselect` ||
|| `style.nodes.<selector>.additional.attrs[].name` | The name of an attribute field displayed in the design form ||
|| `cards.<selector>.additional.attrs[].name` | The name of an attribute field defined separately for a card ||
|| `menu.<selector>.name` | The menu name in the interface ||
|| `menu.<selector>.nodes.<selector>.name` | The node name inside a menu item ||
|#

The table lists the places where the `name` key occurs in a typical manifest. The rule is broader than the table: any `name` key at any nesting level is translated, so review your own manifest in full.

Not translated:

- `block.description` — the block description
- `style.nodes.<selector>.title` — the element label in the style panel, set through the `title` key
- attribute service fields: `placeholder`, `title`, `stubText`
- the `value` entries in `items` lists
- selectors, section codes, node types, and style types

{% note warning "" %}

In the [manifest example](./manifest.md), labels in `style.nodes` are set sometimes through the `name` key and sometimes through `title`. If such a label needs to be translated, set it through the `name` key.

{% endnote %}

An example of a manifest with labels that will be translated:

```php
$manifest = [
    'block' => [
        'name' => 'Title with a separator',
        'section' => ['text'],
    ],
    'nodes' => [
        '.landing-block-node-title' => [
            'name' => 'Title',
            'type' => 'text',
        ],
    ],
    'lang_original' => 'en',
    'lang' => [
        'de' => [
            'Title with a separator' => 'Titel mit Trennlinie',
            'Title' => 'Uberschrift',
        ],
    ],
];
```

## How the System Selects a Translation

The system selects a single language branch from the `lang` array and applies it to the entire manifest:

1. It determines the Bitrix24 language. For the `ru`, `kz`, `by`, and `uz` languages, the `ru` branch is used.
2. If a branch with that code exists in `lang`, it takes the translations from it.
3. If there is no such branch and `lang_original` differs from the Bitrix24 language, it takes the translations from the `en` branch.
4. If no suitable branch exists, it retains the original phrases of the manifest.

Within the selected branch, a translation is substituted only for the phrases present in it as keys. Phrases without a translation remain in the `lang_original` language.

## Which Methods Return a Translated Manifest

#|
|| **Method** | **What It Returns** ||
|| [landing.block.getmanifest](./methods/landing-block-get-manifest.md) | The manifest of a block placed on a page, with translations already substituted. The `lang` key is removed from the response, `lang_original` is retained ||
|| [landing.block.getmanifestfile](./methods/landing-block-get-manifest-file.md) | The original block manifest without translation substitution, together with the `lang_original` and `lang` keys ||
|| [landing.block.getrepository](./methods/landing-block-get-repository.md) | The list of repository blocks, where only the block name is translated. Other labels are not part of this response ||
|#

To see the translations, call [landing.block.getmanifest](./methods/landing-block-get-manifest.md) for a block that has already been added to a page. The substitution language is set by the Bitrix24 language, not by a method parameter.

In the response of this method, check the following:

- the `name` values in `block`, `nodes`, `cards`, `attrs`, and `menu` — they must be in the Bitrix24 language
- the `lang` key — it is absent from the response, the system removes it after substituting the translations
- the `lang_original` key — it is retained exactly as it was passed during registration

If the `name` values arrive in the original language, compare the phrase keys in `lang` with the manifest strings, and check whether `lang` contains a branch for the Bitrix24 language or an `en` branch.

## Permissions and Limitations

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

Localization itself has no dedicated access permissions. Permissions are checked at the level of the methods that register a block and read a manifest, and they are listed on the pages of those methods.

Limitations of localization:

- only the value of the `name` key is translated
- the translation key is the exact string from the manifest, including case, spaces, and punctuation
- a single phrase is translated the same way everywhere in the manifest, and a separate translation for a specific selector cannot be set
- branch codes in `lang` are limited to the codes of languages available in Bitrix24. A branch with a code that is not among the interface languages will not be selected

## What to Consider

- Set `lang_original` to the language the phrases are actually written in. If the value does not match the actual language, the fallback `en` branch will not work.
- Provide the same set of keys in all language branches so that the interface does not mix languages.
- Distinguish labels with different meanings already in the original language. For example, instead of two labels reading "Name", use "Block name" and "Button name".
- Create a separate `en` branch even if the main language of the block is different. It is used as a fallback for languages without their own translation.

## Continue Learning

- [{#T}](./manifest.md)
- [{#T}](./node-types.md)
- [{#T}](./attributes.md)
- [{#T}](./extended-description.md)
- [{#T}](../user-blocks/landing-repo-register.md)
- [{#T}](./methods/landing-block-get-manifest.md)
- [{#T}](../demos/localization.md)
