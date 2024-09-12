# Delete Existing SIP Line voximplant.sip.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- error response is absent

{% endnote %}

{% endif %}

{% include notitle [Scope telephony admin](../../_includes/scope-telephony-admin.md) %}

The method `voximplant.sip.delete` removes an existing SIP line (created by the application). This method is available to the holder of the [permission](https://helpdesk.bitrix24.com/open/18216960/) `Manage numbers - modify - any`.

#|
|| **Parameter** | **Description** ||
|| **CONFIG_ID** | Identifier of the SIP line configuration. ||
|#

## Example

```js
BX24.callMethod(
    "voximplant.sip.delete",
    {
        "CONFIG_ID": 87,
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

Returns 1 upon successful execution or an exception.