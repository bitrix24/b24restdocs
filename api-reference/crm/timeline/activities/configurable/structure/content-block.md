# Configurable Activity Content Block

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Content blocks `ContentBlockDto` are the foundation of the content area of a timeline entry. The application builds the entry content from these blocks: text, links, name-value pairs, and a deadline.

The blocks are passed as an associative array in the `blocks` field: the key is the block identifier that the application sets itself, the value is the block object. The `blocks` array belongs to two objects:

- [`BodyDto`](./body.md) — the content area of a [configurable activity](../index.md); the structure is passed in the `layout` parameter of the [crm.activity.configurable.add](../crm-activity-configurable-add.md) and [crm.activity.configurable.update](../crm-activity-configurable-update.md) methods
- [`RestAppLayoutDto`](./rest-app-layout-dto.md) — a set of additional blocks with which an application enriches someone else's timeline entry using the [crm.activity.layout.blocks.set](../../layout-blocks/crm-activity-layout-blocks-set.md) and [crm.timeline.layout.blocks.set](../../../layout-blocks/crm-timeline-layout-blocks-set.md) methods

Block types and their properties are the same in both cases. The blocks are displayed in the order they are listed in `blocks`.

## General Block Structure

Each block has two fields: `type` — the block type, `properties` — its properties. Each type has its own set of properties, described below.

```json
{
    "type": "text",
    "properties": {
        "value": "The customer confirmed the meeting"
    }
}
```

## How to Choose a Block Type

#|
|| **Type** | **What It Outputs** | **When to Use** ||
|| [`text`](#tekst) | A line of text with formatting | A short value, a label, a comment ||
|| [`largeText`](#dlinnyj-mnogostrochnyj-tekst) | Long text collapsed into a preview | An email, a call transcript, a description ||
|| [`link`](#ssylka) | A link with an action on click | Navigation to a CRM object, an external service, or an app ||
|| [`withTitle`](#blok-s-zagolovkom) | A name-value pair | An entry with a set of fields ||
|| [`lineOfBlocks`](#neskolko-kontent-blokov-v-odnu-stroku) | Several blocks in one line | A name and a phone number side by side, text mixed with links ||
|| [`deadline`](#vybor-krajnego-sroka) | The current activity deadline with the option to change it | An activity with a deadline the user should see and edit ||
|#

## Restrictions and Errors

Only `text`, `link`, and `deadline` blocks can be nested inside `withTitle` and `lineOfBlocks`.

The remaining restrictions depend on where the blocks end up, and so do the calling permissions:

- as part of a [configurable activity](./layout.md) — the [Structure Restrictions](./layout.md#limits) section
- as part of a [set of additional blocks](./rest-app-layout-dto.md) — the [Restrictions](./rest-app-layout-dto.md#limits) section

## Content Block Types

### Text {#tekst}

The `type = text` block outputs a formatted line of text in full, without collapsing. This is the basic block you start building an entry with.

#### Parameters

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **value^*^**
[`textWithTranslation`](./field-types.md#textwithtranslation) | Text the user sees ||
|| **multiline**
[`boolean`](../../../../../data-types.md) | Line break handling. With `true`, `\n` characters are replaced with `<br>`. Default is `false` ||
|| **title**
[`textWithTranslation`](./field-types.md#textwithtranslation) | Tooltip text shown on hovering over the block ||
|| **bold**
[`boolean`](../../../../../data-types.md) | Bold text. Default is `false` ||
|| **size**
[`string`](../../../../../data-types.md) | Text size. Can take values `xs`, `sm`, `md`. Default is `md` ||
|| **color**
[`string`](../../../../../data-types.md) | Text color. Can take values `base_50`, `base_60`, `base_70`, `base_90`. Any other value is rejected by the method with the `ENUM_FIELD` error ||
|| **scope**
[`string`](../../../../../data-types.md) | [Visibility scope](./field-types.md#scope), for example `web` ||
|#

#### Example

Two lines of text, in bold, with a tooltip on hover:

```json
{
    "type": "text",
    "properties": {
        "value": "The customer confirmed the meeting.\nThe meeting is at the office on Tiergartenstraße.",
        "multiline": true,
        "bold": true,
        "size": "md",
        "color": "base_90",
        "title": "Manager's comment"
    }
}
```

This is how the `text` block looks in a timeline entry:

![The text block in a timeline entry](./_images/ContentBlockDto_9.png)

### Long Multiline Text {#dlinnyj-mnogostrochnyj-tekst}

The `type = largeText` block outputs long multiline text and collapses it into a preview.

#### Parameters

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **value^*^**
[`textWithTranslation`](./field-types.md#textwithtranslation) | Text the user sees ||
|| **scope**
[`string`](../../../../../data-types.md) | [Visibility scope](./field-types.md#scope), for example `web` ||
|#

#### Example

```json
{
    "type": "largeText",
    "properties": {
        "value": "Hello! My name is Klaus, I am calling about the request from the website. I checked the stock: both items are available, shipping is possible on Thursday. The customer asks for an invoice to a legal entity and delivery to the door. We agreed to call back once the budget is approved."
    }
}
```

The user can expand the text with the "Show more" button:

![The largeText block collapsed into a preview](./_images/ContentBlockDto_10.png)

### Link {#ssylka}

The `type = link` block outputs a link.

#### Parameters

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **text^*^**
[`textWithTranslation`](./field-types.md#textwithtranslation) | Link text. HTML tags are not supported ||
|| **action^*^**
[`ActionDto`](./action.md) | Action upon clicking the link ||
|| **bold**
[`boolean`](../../../../../data-types.md) | Bold text. Default is `false` ||
|| **scope**
[`string`](../../../../../data-types.md) | [Visibility scope](./field-types.md#scope), for example `web` ||
|#

#### Example

```json
{
    "type": "link",
    "properties": {
     "text": "Open deal",
     "action": {
        "type": "redirect",
        "uri": "/crm/deal/details/123/"
     },
     "bold": true
    }
}
```

![The link block](./_images/ContentBlockDto_15.png)

### Block with Heading {#blok-s-zagolovkom}

The `type = withTitle` block outputs a name-value pair. The value can be another content block.

#### Parameters

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **title^*^**
[`textWithTranslation`](./field-types.md#textwithtranslation) | Title text ||
|| **block^*^**
`ContentBlockDto` | The content block displayed as the value. Blocks of types `text`, `link`, `deadline` are supported ||
|| **inline**
[`boolean`](../../../../../data-types.md) | Display title and value in one line. Default is `false` ||
|| **scope**
[`string`](../../../../../data-types.md) | [Visibility scope](./field-types.md#scope), for example `web` ||
|#

#### Examples

```json
{
    "type": "withTitle",
    "properties": {
        "title": "Heading",
        "block": {
            "type": "text",
            "properties": {
                "value": "Some value"
            }
        }
    }
}
```

![The withTitle block with a text value](./_images/ContentBlockDto_16.png)

```json
{
    "type": "withTitle",
    "properties": {
        "title": "Heading 2",
        "block": {
            "type": "link",
            "properties": {
                "text": "Open deal",
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

![The withTitle block with a link value in one line](./_images/ContentBlockDto_17.png)

### Multiple Content Blocks in One Line {#neskolko-kontent-blokov-v-odnu-stroku}

The `type = lineOfBlocks` block outputs several content blocks in a single line. This is how text with different formatting is combined with links in one line.

#### Parameters

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **blocks^*^**
[`object`](../../../../../data-types.md) | Nested blocks: the key is the block identifier, the value is a `ContentBlockDto` object. No more than 20 blocks, types `text`, `link`, `deadline` are supported ||
|| **scope**
[`string`](../../../../../data-types.md) | [Visibility scope](./field-types.md#scope), for example `web` ||
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
                    "value": "Some text"
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

![Several blocks in one line](./_images/ContentBlockDto_18.png)

### Deadline Selection {#vybor-krajnego-sroka}

The `type = deadline` block shows the activity deadline and allows changing it right in the entry. The block is not displayed in an incoming activity or in an activity without a deadline.

#### Parameters

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **readonly**
[`boolean`](../../../../../data-types.md) | Ban on changing the deadline. Default is `false` — the deadline can be changed right in the entry. Bitrix24 turns the ban on by itself if the activity is completed or the user has no edit access to the object the activity belongs to ||
|| **scope**
[`string`](../../../../../data-types.md) | [Visibility scope](./field-types.md#scope), for example `web` ||
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

![The deadline block](./_images/ContentBlockDto_19.png)

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
