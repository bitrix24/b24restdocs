# Record Header

Header of the [timeline record](../index.md) `HeaderDto`.

## Parameters of the `HeaderDto` Object

{% include [Footnote about parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **title^*^**
[`textWithTranslation`](./field-types.md#textwithtranslation) | Record header ||
|| **titleAction**
[`ActionDto`](./action.md) | Action upon clicking the record header ||
|| **tags**
[`TagDto[]`](#obuekt) | Associative array of objects describing tags ||
|#

## `TagDto` Object

Tag in the timeline record header.

{% note warning %}

No more than two tags are allowed.

{% endnote %}

![](./_images/TagDto_1.png)

### Parameters of the `TagDto` Object

{% include [Footnote about parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **title^*^**
[`textWithTranslation`](./field-types.md#textwithtranslation) | Tag text ||
|| **type^*^**
[`string`](../../../../data-types.md) | Tag type, for example `warning`. Defines its appearance ||
|| **action**
[`ActionDto`](./action.md) | Action upon clicking the tag ||
|| **scope**
[`string`](../../../../data-types.md) | [Scope](./field-types.md#scope), for example `web` ||
|| **hideIfReadonly**
[`boolean`](../../../../data-types.md) | Flag. Hides the tag if the user does not have edit access (default is `false`) ||
|#

Possible values for the **type** field:

- **warning** - Yellow background
- **success** - Green background
- **failure** - Red background
- **primary** - Blue background
- **secondary** - Gray background
- **lavender** - Light purple

![Tag options](./_images/TagDto_2.png)

## Example Object

```json
    "header": {
        "title": "Incoming Call",
        "titleAction": {
            "type": "redirect",
            "uri": "some.url"
        },
        "tags": {
            "status2": {
                "type": "warning",
                "title": "not deciphered"
            }
        }
    },
```

## Continue Learning

- [{#T}](./layout.md)
- [{#T}](./icon.md)
- [{#T}](./body.md)
- [{#T}](./content-block.md)
- [{#T}](./footer.md)
- [{#T}](./menu-item.md)
- [{#T}](./action.md)
- [{#T}](./field-types.md)
- [{#T}](./rest-app-layout-dto.md)
- [{#T}](./examples.md)