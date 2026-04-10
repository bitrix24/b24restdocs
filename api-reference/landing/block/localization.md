# Localization of the Block

The localization of the block is defined in the [manifest file](./manifest.md) and allows for the use of translated labels for different languages.

## Localization Keys in the Manifest

Two keys are used for localization:

- `lang_original` - the original language of the phrases in the manifest
- `lang` - the set of translations by language

In the `lang` array, the keys are the original phrases from the manifest, and the values are the translations of those phrases.

Example:

```php
'lang_original' => 'en',
'lang' => [
    'de' => [
        'Title with a separator on a light background' => 'Titel mit Trennlinie auf hellem Hintergrund',
        'Title' => 'Uberschrift',
        'Text' => 'Text',
        'Button' => 'Schaltflache',
    ],
    'fr' => [
        'Title with a separator on a light background' => 'Titre avec separateur sur fond clair',
        'Title' => 'Titre',
        'Text' => 'Texte',
        'Button' => 'Bouton',
    ],
],
```

## How Localization is Applied

The system iterates through the manifest and searches for translations in the `lang` array for text labels:

- first for the current language of the account
- if there is no translation and `lang_original` differs from the current language of the account, the translation from `en` is used as a fallback

If a suitable translation is not found, the original phrase from the `lang_original` language remains.
