# Custom Fields for Tasks task.item.userfield.getfields

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing examples (should include three examples - curl, js, php)
- missing success response
- missing error response

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.item.userfield.getfields` returns all available fields.

## Parameters

No parameters.

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'task.item.userfield.getfields',
        {},
        function(result)
        {
            console.info(result.data());
            console.log(result);
        }
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}

## List of Fields

#|
|| **Code** / **Type** | **Field** | **Note** ||
|| **ID**
[`int`](../../data-types.md) | Identifier | Read-only ||
|| **ENTITY_ID**
[`string`](../../data-types.md) | Object ||
|| **FIELD_NAME**
[`string`](../../data-types.md) | Code | Immutable ||
|| **USER_TYPE_ID**
[`string`](../../data-types.md) | Data type | Immutable ||
|| **XML_ID**
[`string`](../../data-types.md) | External identifier (XML ID) ||
|| **SORT**
[`int`](../../data-types.md) | Sorting ||
|| **MULTIPLE**
[`char`](../../data-types.md) | Multiple ||
|| **MANDATORY**
[`char`](../../data-types.md) | Required ||
|| **SHOW_FILTER**
[`char`](../../data-types.md) | Show in list filter ||
|| **SHOW_IN_LIST**
[`char`](../../data-types.md) | Show in list ||
|| **EDIT_IN_LIST**
[`char`](../../data-types.md) | Allow user editing ||
|| **IS_SEARCHABLE**
[`char`](../../data-types.md) | Field values participate in search ||
|| **EDIT_FORM_LABEL**
[`string`](../../data-types.md) | Label in edit form ||
|| **LIST_COLUMN_LABEL**
[`string`](../../data-types.md) | Header in list ||
|| **LIST_FILTER_LABEL**
[`string`](../../data-types.md) | Filter label in list ||
|| **ERROR_MESSAGE**
[`string`](../../data-types.md) | Error message ||
|| **HELP_MESSAGE**
[`string`](../../data-types.md) | Help ||
|| **LIST**
[`uf_enum_element`](../../data-types.md) | List elements | Multiple ||
|| **SETTINGS**
[`object`](../../data-types.md) | Additional settings (dependent on type) ||
|#