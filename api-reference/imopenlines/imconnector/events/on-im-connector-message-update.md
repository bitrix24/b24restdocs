# On Message Change Event OnImConnectorMessageUpdate

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- types and requiredness of parameters are not specified

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

This event indicates a message change from the open line. It is essential to call the method **imconnector.send.status.delivery** in response so that the message is marked as successfully changed in the messenger.

#|
|| **Parameter** | **Description** | **Version** ||
|| **CONNECTOR** | Connector ID (this is used to verify if the event belongs to the verifier). | ||
|| **LINE** | ID of the open line. | ||
|| **DATA** | An array of messages, where each message is described by an array of the following format:


```json
{
"im": {
    "chat_id": "845",
    "message_id": "344029"
},
"message": {
    "id": [
     "99"
    ],
    "text": "Sergey \"Pokoev\":\n Test message 55"
},
"chat": {
    "id": "2"
}
}
```
| ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}