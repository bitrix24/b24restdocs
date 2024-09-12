# Make a call to the specified number with automatic pronunciation of the given text voximplant.infocall.startwithtext

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

To access the method, the application must request the call access permission. The permission is specified during [application registration](https://training.bitrix24.com/support/training/course/index.php?COURSE_ID=169&CHAPTER_ID=020052&LESSON_PATH=13643.20053).

#|
|| **Parameter** | **Description** ||
|| **FROM_LINE** | The ID of the line from which the call will be made. A list of available lines can be obtained using the [voximplant.line.get](lines/voximplant-line-get.md) method. ||
|| **TO_NUMBER** | The number to call. ||
|| **TEXT_TO_PRONOUNCE** | The text to be pronounced. ||
|| **VOICE** | The voice to pronounce this text (optional). A list of voices can be obtained using the [voximplant.tts.voices.get](voximplant-tts-voices-get.md) method. ||
|#

## Example

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

{% include [Footnote on examples](../../../_includes/examples.md) %}