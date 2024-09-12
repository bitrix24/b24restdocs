# Get a list of all available outgoing lines voximplant.line.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

{% include notitle [Scope telephony all](../../_includes/scope-telephony-all.md) %}

The method `voximplant.line.get` returns a list of all available outgoing lines. This method is available to the holder of the [access permission](https://helpdesk.bitrix24.com/open/18216960/) `Manage numbers - change - any`.

## Example

```js
BX24.callMethod(
    'voximplant.line.get',
    {},
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.info(result.data());
    }
);
```

{% include [Footnote on examples](../../../../_includes/examples.md) %}