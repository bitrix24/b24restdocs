# Attach a Record to a Completed Call and to the Call Deal telephony.externalCall.attachRecord

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

{% include notitle [Scope telephony all](./_includes/scope-telephony-all.md) %}

The method `telephony.externalCall.attachRecord` attaches a record to a completed call and to the call deal. It should be called after [telephony.externalcall.finish](./telephony-external-call-finish.md) if the record is not ready at the time of the finish call.

Only one record can be attached.

If the method is called multiple times, the next call will overwrite the previous record.

#|
|| **Parameter** / **Type** | **Description** ||
|| **CALL_ID**^*^ 
[`string`](../data-types.md) | The identifier of the call from the method [telephony.externalcall.register](./telephony-external-call-register.md). ||
|| **FILENAME** 
[`string`](../data-types.md) | The name of the file, required. The file name must end with wav or mp3. ||
|| **FILE_CONTENT** 
[`string`](../data-types.md) | Base64-encoded content of the file. Optional. If this parameter is not specified, the method will return the `uploadUrl` parameter - the URL to which the file content can be 'uploaded'. ||
|| **RECORD_URL** 
[`string`](../data-types.md) | A link to the record on the client's server. If specified, an attempt will be made to download the record from the provided address instead of waiting for the record to be uploaded to the client's account. 
During the execution of the method, the account will make one attempt to download the record from the specified address. In case of failure, the method will return an error.

Since the ability to download depends on many factors independent of the account, the use of this parameter is not recommended. ||
|#

{% include [Footnote on parameters](../../_includes/required.md) %}