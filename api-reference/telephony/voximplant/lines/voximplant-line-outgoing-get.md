# Get the current selected line as the default outgoing line voximplant.line.outgoing.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing
- response in case of error is missing

{% endnote %}

{% endif %}

{% include notitle [Scope telephony all](../../_includes/scope-telephony-all.md) %}

The method `voximplant.line.outgoing.get` returns the currently selected line as the default outgoing line. This method is available to the holder of the [permission](https://helpdesk.bitrix24.com/open/18216960/) `Manage numbers - change - any`.

There are no input parameters.

## Example

```js
BX24.callMethod(
    'voximplant.line.outgoing.get',
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

## Response on success

Returns the line identifier (numeric for rented lines, regXXX for Cloud PBXs, sipXXX for Office PBXs).