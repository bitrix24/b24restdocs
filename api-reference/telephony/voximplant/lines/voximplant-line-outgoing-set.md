# Set the Selected Line as the Default Outgoing Line voximplant.line.outgoing.set

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- error response is absent

{% endnote %}

{% endif %}

{% include notitle [Scope telephony admin](../../_includes/scope-telephony-admin.md) %}

The method `voximplant.line.outgoing.set` sets the selected line as the default outgoing line. This method is available to the holder of the [permission](https://helpdesk.bitrix24.com/open/18216960/) `Telephony settings - modify - any`.

#|
|| **Parameter** | **Description** ||
|| **LINE_ID** | The identifier of the line obtained from the method [voximplant.line.get](voximplant-line-get.md) or [voximplant.line.outgoing.get](voximplant-line-outgoing-get.md). ||
|#

## Example

```js
BX24.callMethod(
    "voximplant.line.outgoing.set",
    {
        "LINE_ID": 55,
    },
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

## Success Response

Returns 1 upon successful execution.