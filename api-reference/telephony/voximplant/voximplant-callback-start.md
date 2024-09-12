# Start Callback voximplant.callback.start

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

{% include notitle [Scope telephony all](../_includes/scope-telephony-all.md) %}

The method `voximplant.callback.start` initiates a callback. This method is available to the holder of the [access permission](https://helpdesk.bitrix24.com/open/18216960/) `Outgoing call - Execution - any`.

The callback algorithm works as follows:

1. The client fills out a form on the website, providing their phone number.

2. Upon form submission, an external application triggers the REST API method.

3. The system makes an incoming call to the line specified in the FROM_LINE parameter, according to the line settings, and waits for a connection with the manager. The incoming call is real, following all processing rules. For example, if call forwarding to a mobile phone is enabled on the line, the call will go to the mobile.

4. Once the manager answers, the system speaks the text specified in the TEXT_TO_PRONOUNCE parameter using the voice specified in the VOICE parameter. This is necessary for the manager to understand that they have received a callback, not a regular incoming call.

5. The system makes an outgoing call to the number specified in the TO_NUMBER parameter, and once the client answers, it connects them with the manager.

To access the method, the application must request the access permission for Making Calls (call). The permission is specified during application registration.

#|
|| **Parameter** | **Description** ||
|| **FROM_LINE** | ID of the line from which the call will be made. A list of available lines can be obtained using the [voximplant.line.get](lines/voximplant-line-get.md) method. ||
|| **TO_NUMBER** | The number to call. ||
|| **TEXT_TO_PRONOUNCE** | The text spoken to the manager before the call begins. ||
|| **VOICE** | The voice used to pronounce this text (optional). A list of voices can be obtained using the [voximplant.tts.voices.get](voximplant-tts-voices-get.md) method. ||
|#

## Example

```js
BX24.callMethod(
    'voximplant.callback.start',
    {
        "FROM_LINE": "reg1332",
        "TO_NUMBER": "1911xxxxxxx",
        "TEXT_TO_PRONOUNCE": "You have received a request for a callback, connecting you with the client.",
        "VOICE": "ruinternalfemale"
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

{% include [Footnote on examples](../../../_includes/examples.md) %}