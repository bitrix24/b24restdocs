# Content Block of Configurable Deal

Content blocks `ContentBlockDto` are the main content area of the timeline record. By combining these blocks, various interfaces can be flexibly assembled.

This data structure is used both when creating configurable deals and when enriching timeline records with content blocks (see REST methods for [deals](../../layout-blocks/index.md) | [timeline records](../../../layout-blocks/index.md))

## General Structure of the Block:

```json
{
    "type": "Block Type",
    "properties": {
        ... some properties, varying for each specific block
    }
}
```

## There are several different types of content blocks:

### Text

The simplest block `type = text`, which displays some formatted text.

#### Parameters

{% include [Note on Parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **value^*^**
[`textWithTranslation`](./field-types.md) | The text that will be displayed ||
|| **multiline**
[`boolean`](../../../../data-types.md) | Should line breaks be processed? If true, `\n` characters will be replaced with `<br>`. Default is `false` ||
|| **title**
[`textWithTranslation`](./field-types.md#textwithtranslation) | Title attribute ||
|| **bold**
[`boolean`](../../../../data-types.md) | Should the text be bold? Default is `false` ||
|| **size**
[`string`](../../../../data-types.md) | Text size. Can take values `xs`, `sm`, `md` (default is `md`) ||
|| **color**
[`string`](../../../../data-types.md) | Text color. Can take values `base_50`, `base_60`, `base_70`, `base_90`, `green`, `purple` ||
|| **scope**
[`string`](../../../../data-types.md) | [Visibility](./field-types.md#scope), for example `web` ||
|#

#### Example

```json
{
    "icon": {
        "code": "info"
    },
    "header": {
        "title": "Information Message"
    },
    "body": {
        "logo": {
            "code": "notification"
        },
        "blocks": {
            "text": {
                "type": "text",
                "properties": {
                    "value": "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."
                }
            }
        }
    }
}
```

![Text](./_images/ContentBlockDto_9.png)

### Long Multiline Text

The block `type = largeText`, which allows displaying long multiline texts that will automatically collapse to a preview.

#### Parameters

{% include [Note on Parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **value^*^**
[`textWithTranslation`](./field-types.md#textwithtranslation) | The text that will be displayed ||
|| **scope**
[`string`](../../../../data-types.md) | [Visibility](./field-types.md#scope), for example `web` ||
|#

#### Example

Long text collapsed under "Show More".

```json
{
    "icon": {
        "code": "info"
    },
    "header": {
        "title": "Information Message"
    },
    "body": {
        "logo": {
            "code": "notification"
        },
        "blocks": {
            "text": {
                "type": "largeText",
                "properties": {
                    "value": "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum. Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."
                }
            }
        }
    }
}
```

![Long Text](./_images/ContentBlockDto_10.png)

### Link

The block `type = link`, which displays a link.

#### Parameters

{% include [Note on Parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **text^*^**
[`textWithTranslation`](./field-types.md#textwithtranslation) | The text that will be displayed. HTML tags are not supported ||
|| **action^*^**
[`ActionDto`](./action.md) | Action upon clicking the link ||
|| **bold**
[`boolean`](../../../../data-types.md) | Should the text be bold? Default is `false` ||
|| **scope**
[`string`](../../../../data-types.md) | [Visibility](./field-types.md#scope), for example `web` ||
|#

#### Example

```json
{
    "type": "link",
    "properties": {
     "text": "Open Deal",
     "action": {
        "type": "redirect",
        "uri": "/crm/deal/details/123/"
     },
     "bold": true
    }
}
```

### Block with Title

The block `type = withTitle` allows displaying a pair of title-value. Another content block can be used as the value.

#### Parameters

{% include [Note on Parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **title^*^**
[`textWithTranslation`](./field-types.md#textwithtranslation) | Title text ||
|| **block^*^**
[`ContentBlockDto`](content-block.md) | Content block that serves as the value. Blocks of types `text`, `link`, `deadline` are supported ||
|| **inline**
[`boolean`](../../../../data-types.md) | Should the title and value be displayed in one line? Default is `false` ||
|| **scope**
[`string`](../../../../data-types.md) | [Visibility](./field-types.md#scope), for example `web` ||
|#

#### Examples

```json
{
    "type": "withTitle",
    "properties": {
        "title": "Title",
        "block": {
            "type": "text",
            "properties": {
                "value": "Some Value"
            }
        }
    }
}
```

```json
{
    "type": "withTitle",
    "properties": {
        "title": "Title 2",
        "block": {
            "type": "link",
            "properties": {
                "text": "Open Deal",
                "action": {
                    "type": "redirect",
                    "uri": "/crm/deal/details/123/"
                }
            }
        },
        "inline": true
    }
}
```

### Multiple Content Blocks in One Line

The block `type = lineOfBlocks` allows displaying multiple content blocks of type text or link in one line. This enables displaying text with different formatting mixed with links in a single line.

#### Parameters

{% include [Note on Parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **blocks^*^**
[`ContentBlockDto[]`](content-block.md) | Associative array of content blocks. Blocks of types `text`, `link`, `deadline` are supported ||
|| **scope**
[`string`](../../../../data-types.md) | [Visibility](./field-types.md#scope), for example `web` ||
|#

#### Examples

```json
{
    "type": "lineOfBlocks",
    "properties": {
        "blocks": {
            "text": {
                "type": "text",
                "properties": {
                    "value": "Some Text"
                }
            },
            "link": {
                "type": "link",
                "properties": {
                    "text": "link",
                    "action": {
                        "type": "redirect",
                        "uri": "/crm/deal/details/123/"
                    }
                }
            },
            "boldText": {
                "type": "text",
                "properties": {
                    "value": "bold text",
                    "bold": true
                }
            }
        }
    }
}
```

### Deadline Selection

The block `type = deadline` displays the current deadline value with the ability to quickly change it. The block will not be shown if added to an incoming deal or a deal without a deadline.

#### Parameters

{% include [Note on Parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **readonly**
[`boolean`](../../../../data-types.md) | Allow changing the deadline? Default is `false`. If the user does not have permission to modify the entity to which the deal relates, or if the deal is completed, then `readonly = true` regardless of the settings passed ||
|| **scope**
[`string`](../../../../data-types.md) | [Visibility](./field-types.md#scope), for example `web` ||
|#

#### Examples

```json
{
    "type": "deadline",
    "properties": {
        "readonly": false
    }
}
```

## Continue Learning

- [{#T}](../../layout-blocks/index.md)
- [{#T}](../../../layout-blocks/index.md)
- [{#T}](./layout.md)
- [{#T}](./icon.md)
- [{#T}](./header.md)
- [{#T}](./body.md)
- [{#T}](./footer.md)
- [{#T}](./menu-item.md)
- [{#T}](./action.md)
- [{#T}](./field-types.md)
- [{#T}](./rest-app-layout-dto.md)
- [{#T}](./examples.md)