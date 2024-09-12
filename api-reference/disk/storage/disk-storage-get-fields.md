# Get Description of Storage Fields disk.storage.getfields

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing examples (should include three examples - curl, js, php)
- missing response in case of error
- missing response in case of success

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.storage.getfields` returns the description of storage fields.

- `TYPE` — type of the field;
- `USE_IN_FILTER` — whether the field can be used for filtering the selection;
- `USE_IN_SHOW` — whether this field is available when receiving a response.

## Parameters

No parameters.

## Example

```js
BX24.callMethod(
    "disk.storage.getfields",
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
{% include [Note about examples](../../../_includes/examples.md) %}