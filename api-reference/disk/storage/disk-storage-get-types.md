# Get Storage Types disk.storage.gettypes

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing examples (there should be three examples - curl, js, php)
- missing error response

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.storage.gettypes` returns a list of storage types.

## Parameters

No parameters.

## Example

```js
BX24.callMethod(
    "disk.storage.gettypes",
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

## Successful Response

> 200 OK

```json
"result": [
    "user", //user storage
    "common", //common documents storage
    "group" //social groups storage
]
```