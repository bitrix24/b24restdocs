# Record Header

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- are the types other than standard specified correctly?

{% endnote %}

{% endif %}

## HeaderDto

The header of the timeline record.

#|
|| **Field** | **Description** ||
|| **title^*^**
[textWithTranslation](./field-types.md) | The title of the record ||
|| **titleAction**
[ActionDto](./action.md) | Action when the record title is clicked ||
|| **tags**
[TagDto](#tagdto) | No more than two tags are allowed ||
|#

{% include [Footnote about parameters](../../../../../_includes/required.md) %}

## TagDto

A tag in the timeline record header.

![](./_images/TagDto_1.png)

#|
|| **Field** | **Description** ||
|| **title^*^**
[textWithTranslation](./field-types.md) | The text of the tag ||
|| **type^*^**
[`string`](../../../../data-types.md) | The type of the tag. Determines its appearance. ||
|| **action**
[ActionDto](./action.md) | Action when the tag is clicked ||
|| **scope**
[`string`](../../../../data-types.md) | Where to display it. ||
|| **hideIfReadonly**
[`boolean`](../../../../data-types.md) | Hide the tag if the user does not have edit access. Defaults to `=false`. ||
|#

{% include [Footnote about parameters](../../../../../_includes/required.md) %}

Possible values for the **type** field:

![](./_images/TagDto_2.png)

- **success** - Green background
- **failure** - Red background
- **warning** - Yellow background
- **primary** - Blue background
- **secondary** - Gray background

### Example

```json
{
    "title": "Attention! Important!",
    "type": "warning"
}
```