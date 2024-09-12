# Get an Array of Available Voices for Text-to-Speech voximplant.tts.voices.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

{% include notitle [Scope telephony all](../_includes/scope-telephony-all.md) %}

The method `voximplant.tts.voices.get` returns an array of available voices for text-to-speech in the format of voice ID => voice name. The method has no restrictions on [access permissions](https://helpdesk.bitrix24.com/open/18216960/).

There are no incoming parameters.

## Example

```javascript
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

{% include [Footnote on examples](../../../_includes/examples.md) %}