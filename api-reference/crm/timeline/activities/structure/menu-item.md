# Dropdown Menu at the Bottom

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- are the types other than standard correctly specified?

{% endnote %}

{% endif %}

Items in the dropdown menu at the bottom of the timeline entry.

#|
|| **Field** | **Description** | **Additional** ||
|| **title^*^**
[`textWithTranslation`](./field-types.md) | Button text | Required ||
|| **action^*^**
[`ActionDto`](./action.md) | Action to be performed when the button is clicked. | Required ||
|| **scope**
[`string`](../../../../data-types.md) | Where to display | ||
|| **hideIfReadonly**
[`boolean`](../../../../data-types.md) | Hide if the user does not have edit access. | Defaults to `false`. ||
|#

{% include [Footnote on parameters](../../../../../_includes/required.md) %}

## Example

```json
{
    "title": "Confirm Process",
    "action": {
        "type": "restEvent",
        "id": "confirmProcess",
        "animationType": "loader"
    },
    "scope": "web",
    "hideIfReadonly": true
}
```