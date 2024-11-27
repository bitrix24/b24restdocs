# Add a new resource calendar.resource.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.resource.add` adds a new resource and takes an array of parameters as input.

#| 
|| **Parameter** / **Type** | **Description** ||
|| **name**^*^ 
[`string`](../data-types.md) | Resource name. ||
|#

{% include [Parameter notes](../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod("calendar.resource.add",
        {
            name: 'My resource title'
        }
    );
    ```

{% endlist %}

{% include [Example notes](../../_includes/examples.md) %}

## Response in case of success

Returns the ID of the added resource.