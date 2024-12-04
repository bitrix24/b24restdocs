# Get an array of available voices for text-to-speech voximplant.tts.voices.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

{% include notitle [Scope telephony all](../_includes/scope-telephony-all.md) %}

The method `voximplant.tts.voices.get` returns an array of available voices for text-to-speech in the format voice ID => voice name. The method has no restrictions on [access permissions](https://helpdesk.bitrix24.com/open/18216960/).

There are no incoming parameters.

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'voximplant.tts.voices.get',
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

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}