# Add transcription of the call to telephony.call.attachTranscription

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

{% include notitle [Scope telephony all](./_includes/scope-telephony-all.md) %}

The method `telephony.call.attachTranscription` adds a transcription of the call.

#|
|| **Parameter** / **Type** | **Description** ||
|| **CALL_ID** 
[`string`](../data-types.md) | Call identifier. ||
|| **COST** 
[`float`](../data-types.md) | Cost of the transcription. ||
|| **COST_CURRENCY** 
[`string`](../data-types.md) | Currency of the transcription cost. ||
|| **MESSAGES** | Transcription of the call. An array of `TranscriptMessage` objects. ||
|#

## Fields of the TranscriptMessage class

#|
|| **Parameter** / **Type** | **Description** ||
|| **SIDE** 
[`string`](../data-types.md) | Participant of the conversation. Possible values: 
- User - portal user, 
- Client - external participant. ||
|| **START_TIME** 
[`int`](../data-types.md) | Start time of the phrase in seconds from the beginning of the conversation. ||
|| **STOP_TIME** 
[`int`](../data-types.md) | End time of the phrase in seconds from the beginning of the conversation. ||
|| **MESSAGE** 
[`string`](../data-types.md) | Text of the phrase. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    var callId = '<Id of the call>';
    var messages = [
        {
            SIDE: "User",
            START_TIME: 1,
            STOP_TIME: 3,
            MESSAGE: "Good afternoon, how can I help you?"
        },
        {
            SIDE: "Client",
            START_TIME: 4,
            STOP_TIME: 8,
            MESSAGE: "Hello, do you sell vacuum cleaners?"
        },
        {
            SIDE: "User",
            START_TIME: 9,
            STOP_TIME: 11,
            MESSAGE: "Unfortunately, no."
        },
        {
            SIDE: "Client",
            START_TIME: 11,
            STOP_TIME: 13,
            MESSAGE: "I see, goodbye."
        },
        {
            SIDE: "User",
            START_TIME: 13,
            STOP_TIME: 15,
            MESSAGE: "Goodbye."
        },
    ];

    BX24.callMethod(
        "telephony.call.attachTranscription",
        {
            CALL_ID: callId,
            MESSAGES: messages
        },
        function(response)
        {
            console.log(response.data())
        }
    );
    ```

{% endlist %}



{% include [Footnote on examples](../../_includes/examples.md) %}