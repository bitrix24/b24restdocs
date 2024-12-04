# Set the selected SIP line as the default outgoing line voximplant.line.outgoing.sip.set

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

{% include notitle [Scope telephony admin](../../_includes/scope-telephony-admin.md) %}

The method `voximplant.line.outgoing.sip.set` sets the selected SIP line as the default outgoing line. This method is available to the holder of the [access permissions](https://helpdesk.bitrix24.com/open/18216960/) `Manage numbers - change - any`.

#|
|| **Parameter** | **Description** ||
|| **CONFIG_ID** | Identifier of the SIP line configuration. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "voximplant.line.outgoing.sip.set",
        {
            "CONFIG_ID": 57,
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

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}

## Response on success

Returns 1 on successful execution.