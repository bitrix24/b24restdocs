# Get Description of Fields for disk.file.getfields

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing examples (there should be three examples - curl, js, php)
- missing response in case of success
- missing response in case of error

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.file.getfields` returns the description of file fields.

- `TYPE` — field type;
- `USE_IN_FILTER` — whether the field can be used for filtering the selection;
- `USE_IN_SHOW` — whether this field is available when receiving a response.

## Parameters

No parameters

## Example

```js
BX24.callMethod(
    "disk.file.getfields",
    {},
    function (result)
    {
        if (result.error())
            console.error(result.error());
        else
            console.dir(result.data());
    }
);
```
{% include [Footnote on examples](../../../_includes/examples.md) %}