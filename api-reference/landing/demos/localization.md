# Template Localization

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Localization determines how a template displays its title, description, and other key phrases in the Bitrix24 language. This is set during the template registration through [landing.demos.register](./landing-demos-register.md).

This article explains the structure of the `lang` and `lang_original` parameters. It also describes how the system selects translations and how to verify the saved localization through [landing.demos.getList](./landing-demos-get-list.md).

## How to Add Localization

1. Prepare the original phrases of the template in one language.
2. Pass the code of that language in `lang_original`.
3. Provide the translations in `lang`.
4. After registration, check the `LANG` field using the [landing.demos.getList](./landing-demos-get-list.md) method.

## Which Parameters Control Localization

**lang_original.** The code of the original language of the phrases in the template. This parameter is passed in [landing.demos.register](./landing-demos-register.md). Example value: `ru`.

**lang.** An object of translations by language codes. This parameter is passed in [landing.demos.register](./landing-demos-register.md). Within each language, the original phrase serves as the key, and the translation of that phrase is the value.

After registration, the system saves both parameters in the `LANG` field.

The structure of the array may look like this:

- `lang_original` — the code of the original language of the phrases
- `lang.en` — translations for the English language
- `lang.de` — translations for the German language, if the template is used in German

For example:

```json
{
    "lang_original": "ru",
    "lang": {
        "en": {
            "Template title": "Template title"
        },
        "de": {
            "Template title": "Vorlagenname"
        }
    }
}
```

## What Can Be Localized

In this example, the title and description are included in the localization array:

```json
{
    "lang_original": "ru",
    "lang": {
        "en": {
            "Template title": "Template title",
            "Template description": "Template description"
        },
        "de": {
            "Template title": "Vorlagenname",
            "Template description": "Vorlagenbeschreibung"
        }
    }
}
```

The composition of keys depends on the template. The localization array typically includes the title, description, and string values in `ADDITIONAL_FIELDS` or similar fields of the template.

## How the System Selects a Translation

When rendering the template through the `landing.demo` component, the system first looks for translations for the Bitrix24 language.

For the languages `ru`, `kz`, `by`, and `uz`, the `ru` branch is used.

If there is no translation for the Bitrix24 language and `lang_original` differs from it, the `en` branch is used.

If no suitable translation is found, the original phrases of the template remain.

## How to Verify Localization

After registration, call [landing.demos.getList](./landing-demos-get-list.md) and request the `LANG` field. In the response, check:

- whether the localization object has been saved
- which languages are present in `lang`
- which language is specified in `lang_original`

Example response from the method:

```json
{
    "ID": "11",
    "XML_ID": "demo_localization_test",
    "TITLE": "Template title",
    "DESCRIPTION": "Template description",
    "TYPE": "page",
    "TPL_TYPE": "S",
    "LANG": {
        "lang": {
            "en": {
                "Template title": "Template title",
                "Template description": "Template description"
            },
            "de": {
                "Template title": "Vorlagenname",
                "Template description": "Vorlagenbeschreibung"
            }
        },
        "lang_original": "ru"
    }
}
```

The `TITLE` and `DESCRIPTION` fields are returned in the form they were saved during registration. Translations are stored separately in `LANG`.

## What to Consider

- The value of `lang_original` must match the original language of the phrases in the template.
- For each language, provide the same set of keys.
- If the template is displayed in Bitrix24 in different languages, prepare the localization array before registration.
- The [landing.demos.getList](./landing-demos-get-list.md) method helps verify that the localization has been saved in the expected structure.

Localization in `landing.demos.*` is stored separately from the main fields of the template. Therefore, after registration, it is important to check not only the `TITLE` and `DESCRIPTION` but also the contents of the `LANG` field.

## Continue Learning

- [How to Prepare a Custom Template](./introduction.md)
- [Register a Template in the Site Creation Wizard](./landing-demos-register.md)
- [Get a List of Registered Templates](./landing-demos-get-list.md)
- [Overview of Custom Template Methods](./index.md)