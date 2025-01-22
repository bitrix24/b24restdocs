# Make a call to the specified number with automatic text pronunciation voximplant.infocall.startwithtext

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

{% include notitle [Scope telephony all](../_includes/scope-telephony-all.md) %}

The method `voximplant.infocall.startwithtext` makes a call to the specified number with automatic pronunciation of the given text. This method is available to the holder of the [access permission](https://helpdesk.bitrix24.com/open/18216960/) `Outgoing call - Execution - any`.

To access the method, the application must request the access permission for Making calls (call). The permission is specified during [application registration](../../app-installation/index.md).

#|
|| **Parameter** | **Description** ||
|| **FROM_LINE** | ID of the line from which the call will be made. A list of available lines can be obtained using the method [voximplant.line.get](lines/voximplant-line-get.md). ||
|| **TO_NUMBER** | The number to call. ||
|| **TEXT_TO_PRONOUNCE** | The text to be pronounced. ||
|| **VOICE** | The voice to pronounce this text (optional). A list of voices can be obtained using the method [voximplant.tts.voices.get](voximplant-tts-voices-get.md). ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'voximplant.infocall.startwithtext',
        {
            "FROM_LINE": "reg1332",
            "TO_NUMBER": "+1911xxxxxxx",
            "PRONOUNCE": "Good afternoon. Your request has been completed.",
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

{% include [Footnote on examples](../../../_includes/examples.md) %}