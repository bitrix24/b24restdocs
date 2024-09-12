# Bind Data to the app.option.set Method

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- required parameters are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `app.option.set` method binds data to the application.

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **options**
[`array`](../../data-types.md) | An array where the key is the name of the property to be saved, and the value is the property value. If a value with a new key is provided, the method will save it; if it exists, it will update it. | ||
|#

## Examples

```js
CRest::call(
    'app.option.set',
    [
        "options"=>[
            'data' => 'value',
            'data2' => 'value2',
        ]
    ]
);
```

```js
CRest::call(
    'app.option.set',
    [
        "options"=>[
            'data' => 'NewValue',
        ]
    ]
),
```

{% include [Footnote on examples](../../../_includes/examples.md) %}