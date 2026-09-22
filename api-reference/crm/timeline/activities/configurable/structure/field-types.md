# Field Types

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The fields of the [configurable activity layout](./layout.md) use two special value types: text with translations and scope. The types of the other fields are listed in the tables on the layout pages.

## textWithTranslation {#textwithtranslation}

Text that can be passed in several languages at once.

If translations are not needed, pass a string — it is displayed as is. If the application has users with different interface languages, pass an object instead of a string: the key is a language code, the value is the text in that language.

```json
{
    "de": "Speichern",
    "en": "Save"
}
```

Rules for the translations object:

- keys must be language codes for languages installed in Bitrix24; otherwise, the `WRONG_LANG` error is returned
- the value of each key is a string
- the object must not be empty: it passes validation, but the record ends up with empty text

Bitrix24 stores the whole object and picks the right variant when the record is displayed — based on the interface language of the user viewing the timeline. If there is no translation for their language, Bitrix24 uses English, and if there is no English either — the first value of the object.

The type is used in the record header, tags and badges, text blocks and links, footer buttons, menu items and sections. A working example is in the [Multi-language Entry](./examples.md#multilang-card) section.

## scope {#scope}

Scope defines on which devices a record element is displayed.

#|
|| **Value** | **Where the Element Appears** ||
|| field not passed | Everywhere ||
|| `web` | Only in the browser ||
|| `mobile` | Only in the mobile app ||
|#

The `scope` field is available in [content blocks](./content-block.md), [footer buttons](./footer.md), and [menu items](./menu-item.md). For buttons and menu items, Bitrix24 rejects any other value with the `ENUM_FIELD` error; for content blocks, the value is not validated.

## Continue Learning

- [{#T}](./layout.md)
- [{#T}](./icon.md)
- [{#T}](./header.md)
- [{#T}](./body.md)
- [{#T}](./content-block.md)
- [{#T}](./footer.md)
- [{#T}](./menu-item.md)
- [{#T}](./action.md)
- [{#T}](./rest-app-layout-dto.md)
- [{#T}](./examples.md)
