# Main Content Area

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- are the types other than standard specified correctly?

{% endnote %}

{% endif %}

## BodyDto

The main content area of the timeline entry.

#|
|| **Field** | **Description** | **Additional** ||
|| **logo^*^**
[`LogoDto`](#logodto) | Logo of the entry | Required. ||
|| **blocks**
[`ContentBlockDto`](#contentblockdto) | Associative array of objects describing content blocks. | The array must contain at least one element and no more than 20 elements. ||
|#

{% include [Footnote on parameters](../../../../../_includes/required.md) %}

## LogoDto

Logo of the timeline entry.

#|
|| **Field** | **Description** | **Additional** ||
|| **code^*^**
[`string`](../../../../data-types.md) | Logo code | Required. A list of available codes can be obtained using the method [crm.timeline.logo.list](.). ||
|| **action**
[`ActionDto`](./action.md) | Action when clicking on the logo. | ||
|#

{% include [Footnote on parameters](../../../../../_includes/required.md) %}

## ContentBlockDto

Content blocks of the main content area of the timeline entry. By combining these blocks, various interfaces can be flexibly assembled.

General structure of a block:

```json
{
    "type": "Block type",
    "properties": {
        ... some properties, varying for each specific block
    }
}
```

There are several different types of content blocks:

### Text

The simplest block `type = text`, which displays some formatted text.

#|
|| **Field** | **Description** | **Additional** ||
|| **value^*^**
[`textWithTranslation`](./field-types.md) | Text to be displayed | Required. ||
|| **multiline**
[`boolean`](../../../../data-types.md) | Should line breaks be processed | If true, `\n` characters will be replaced with `<br>`. Default is `false`. ||
|| **title**
[`textWithTranslation`](./field-types.md) | Title attribute | ||
|| **bold**
[`boolean`](../../../../data-types.md) | Should the text be bold? | Default is `false`. ||
|| **size**
[`string`](../../../../data-types.md) | Text size | Can take values xs, sm, md (the last one is used by default). ||
|| **color**
[`string`](../../../../data-types.md) | Text color | Can take values base_50, base_60, base_70, base_90, green ||
|| **scope**
[`string`](../../../../data-types.md) | Where to display. | ||
|#

{% include [Footnote on parameters](../../../../../_includes/required.md) %}

**Example:**

```json
{
    "type": "text",
    "properties": {
     "value": "Hello!\nWe are starting.",
     "multiline": true,
     "bold": true,
     "color": "base_90"
    }
}
```

### Long Multiline Text

Block `type = largeText`, allowing the display of long multiline texts that will automatically collapse to a preview.

#|
|| **Field** | **Description** | **Additional** ||
|| **value^*^**
[`textWithTranslation`](./field-types.md) | Text to be displayed | Required. ||
|| **scope**
[`string`](../../../../data-types.md) | Where to display | ||
|#

{% include [Footnote on parameters](../../../../../_includes/required.md) %}

**Example:**

```json
{
    "type": "largeText",
    "properties": {
     "value": "Hello!\nWe are starting.\nWe are continuing.\nWe are still working on this.\nWe are continuing.\nWe are close to the result.\nGoodbye."
    }
}
```

### Link

Block `type = link`, which displays a link.

#|
|| **Field** | **Description** | **Additional** ||
|| **text^*^**
[`textWithTranslation`](./field-types.md) | Text to be displayed. HTML tags are not supported. | Required. ||
|| **action^*^**
[`ActionDto`](./action.md) | Action when clicking on the link. | Required. ||
|| **bold**
[`boolean`](../../../../data-types.md) | Should the text be bold? | Default is `false`. ||
|| **scope**
[`string`](../../../../data-types.md) | Where to display | ||
|#

{% include [Footnote on parameters](../../../../../_includes/required.md) %}

**Example:**

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

### Block with Title

Block `type = withTitle` allows displaying a pair of title-value. Another content block can be used as the value.

#|
|| **Field** | **Description** | **Additional** ||
|| **title^*^**
[`textWithTranslation`](./field-types.md) | Title text | Required. ||
|| **block^*^**
[`ContentBlockDto`](#contentblockdto) | Content block that serves as the value. Blocks of types text, link, deadline are supported. | Required. ||
|| **inline**
[`boolean`](../../../../data-types.md) | Should the title and value be displayed in one line? | Default is `false`. ||
|| **scope**
[`string`](../../../../data-types.md) | Where to display. | ||
|#

{% include [Footnote on parameters](../../../../../_includes/required.md) %}