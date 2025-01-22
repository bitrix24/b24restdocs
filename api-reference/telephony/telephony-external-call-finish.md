# Finish Call and Record It in Telephony Statistics telephony.externalcall.finish

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- response in case of error is missing

{% endnote %}

{% endif %}

{% include notitle [Scope telephony all](./_includes/scope-telephony-all.md) %}

The method `telephony.externalcall.finish` ends the call, records it in the statistics, and hides the call card from the user.

#|
|| **Parameter** / **Type** | **Description** ||
|| **CALL_ID**^*^ 
[`string`](../data-types.md) | Call identifier from the method [telephony.externalcall.register](telephony-external-call-register.md). ||
|| **USER_ID**^*^ 
[`int`](../data-types.md) | User identifier. The responsible person in the lead will be changed to the provided USER_ID. The responsible person will only change for leads created automatically when calling the method [telephony.externalcall.register](telephony-external-call-register.md). For existing leads, the responsible person does not change. ||
|| **DURATION**^*^ 
[`int`](../data-types.md) | Duration in seconds. ||
|| **COST** 
[`double`](../data-types.md) | Cost of the call. ||
|| **COST_CURRENCY** 
[`string`](../data-types.md) | Currency in which the call cost is specified. The list of currencies can be obtained using the method `crm.currency.list`. ||
|| **STATUS_CODE** 
[`string`](../data-types.md) | Call code (see [call codes table](#call-failed-code)) ||
|| **FAILED_REASON** 
[`string`](../data-types.md) | Reason for the failed call. ||
|| **RECORD_URL** 
[`string`](../data-types.md) | URL of the file (preferably mp3, flac is possible) with the call recording. The account will make two attempts to download the recording from the specified address. If unsuccessful, the recording will not be attached, and no error notification will be sent.

{% note info %}

This parameter is deprecated and retained solely for backward compatibility. Its use is strongly discouraged. To upload the call recording, use the method [telephony.externalCall.attachRecord](telephony-external-call-attach-record.md).

{% endnote %}
||
|| **VOTE** 
[`int`](../data-types.md) | User rating of the call (if the PBX supports call rating functionality). ||
|| **ADD_TO_CHAT** 
[`int`](../data-types.md) | Whether to add a message about the completed call to the B24 employee's chat (Possible values are 0 or 1, default is 0). ||
|#

{% include [Footnote on parameters](../../_includes/required.md) %}

{% note info %}

Instead of USER_ID, USER_PHONE_INNER can also be used.

{% endnote %}

## Call Codes Table (CALL_FAILED_CODE) {#call-failed-code}

#|
|| **Code** | **Description** ||
|| `200` | Successful call ||
|| `304` | Missed call ||
|| `603` | Declined ||
|| `603-S` | Call canceled ||
|| `403` | Forbidden ||
|| `404` | Invalid number ||
|| `486` | Busy ||
|| `484` | This direction is unavailable ||
|| `503` | This direction is unavailable ||
|| `480` | Temporarily unavailable ||
|| `402` | Insufficient funds in the account ||
|| `423` | Blocked ||
|| `OTHER` | Undefined ||
|#

## Response in Case of Success

The method returns an array similar to the method [voximplant.statistic.get](voximplant-statistic-get.md).