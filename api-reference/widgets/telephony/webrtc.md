# WebRTC

Bitrix24 allows you to embed an external WebRTC client into the web version of the product. To do this, you need to follow several steps:

1. Upload your WebRTC client to a [special location](../universal/background-worker.md) for embedding widgets `PAGE_BACKGROUND_WORKER`;
2. In case of an incoming call, register it using the standard telephony integration method [{#T}](../../telephony/telephony-external-call-register.md), which will also display the standard call detail form to the user;
3. Manage the state and buttons of the call detail form using [special js methods](../ui-interaction/page-background-worker/index.md) available for the widget handler `PAGE_BACKGROUND_WORKER`;
4. After the call ends, notify Bitrix24 about it using the method [{#T}](../../telephony/telephony-external-call-finish.md).

Other functionalities, such as uploading call recordings to Bitrix24, can also be implemented using the corresponding [telephony integration methods](../../telephony/index.md) without referring to the WebRTC client.