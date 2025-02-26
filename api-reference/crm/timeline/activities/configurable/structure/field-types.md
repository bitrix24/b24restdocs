# Field Types

Available field types for configuring [your types of activities](../../types/index.md).

## textWithTranslation

Text with translation support.

Fields of type `textWithTranslation` support multilingualism. In its simplest form, you just need to pass a string into it. This string will be used as the value.

If support for different interface languages is required, a non-empty array can be passed as the field value, where the keys are the language codes and the values are the text in that language, for example:

```json
{
    "de": "Speichern",
    "en": "Save"
}
```

If a translation for the current language is not found, English will be used. If the translation in English is also not found, the first element of the array will be used.

## Scope

Visibility. Where to display the block.

Some timeline entry blocks have a `scope` parameter. If it is filled, the visibility of the corresponding block will be limited to a specific type of device.

Possible values:

- **Not filled** - the block will be shown everywhere
- **web** - the block will be shown only in the browser
- **mobile** - the block will be shown only in the mobile application