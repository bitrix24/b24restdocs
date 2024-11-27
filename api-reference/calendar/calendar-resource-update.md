# Change resource calendar.resource.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.resource.update` modifies a resource.

#| 
|| **Parameter** | **Description** ||
|| **resourceId** | Resource identifier. ||
|| **name**^*^ | Resource name. ||
|#

{% include [Footnote about parameters](../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod("calendar.resource.update",
        {
            resourceId: 325,
            name: 'Changed Resource Name'
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../_includes/examples.md) %}

## Response on success

Returns the ID of the modified section.