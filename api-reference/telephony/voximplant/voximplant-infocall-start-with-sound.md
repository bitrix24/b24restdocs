# Make a call to the specified number while playing an mp3 file from the URL voximplant.infocall.startwithsound

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly

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

The method `voximplant.infocall.startwithsound` makes a call to the specified number while playing an mp3 file from the URL. This method is available to the holder of the [access permission](https://helpdesk.bitrix24.com/open/18216960/) `Outgoing call - Execution - any`.

To access the method, the application must request the access permission `Making calls (call)`. The permission is specified during [application registration](../../app-installation/index.md).

#|
|| **Parameter** | **Description** ||
|| **FROM_LINE** | The ID of the line from which the call will be made. A list of available lines can be obtained using the method [voximplant.line.get](lines/voximplant-line-get.md). ||
|| **TO_NUMBER** | The number to call. ||
|| **URL** | The address of the mp3 recording to be played. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'voximplant.infocall.startwithsound',
        {
            "FROM_LINE": "reg1332",
            "TO_NUMBER": "+1911xxxxxxx",
            "URL": "http://your.domain/path/file.mp3",
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

{% include [Footnote about examples](../../../_includes/examples.md) %}