# Field Types

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed to meet writing standards

{% endnote %}

{% endif %}

## textWithTranslation

Text with translation support.

Fields marked with the type *textWithTranslation* support multilingualism. In a simple case, it is enough to pass a string into them. This string will be used as the value.

If support for different interface languages is required, a non-empty array can be passed as the field value, where the keys are the language codes and the values are the text in that language, for example:

```json
{
    "en": "Save"
}
```

If a translation for the current language is not found, English will be used. If English is also not found, the first element of the array will be used.

## Scope

Where to display the block.

Some timeline entry blocks have a **scope** parameter. If it is filled, the visibility of the corresponding block will be limited to a specific type of device.

Possible values:

- **Not filled** - the block will be shown everywhere.
- **web** - the block will be shown only in the browser.
- **mobile** - the block will be shown only in the mobile application.