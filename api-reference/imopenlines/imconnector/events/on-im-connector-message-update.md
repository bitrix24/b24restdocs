# On Message Change Event OnImConnectorMessageUpdate

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- types and required parameters are not specified

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

This event indicates a message change in the Open Channels. It is essential to call the method **imconnector.send.status.delivery** in response, so the message is marked as successfully changed in the messenger.

#| 
|| **Parameter** | **Description** | **Available since** ||
|| **CONNECTOR** | Connector ID (used to verify if this event belongs to the verifier). | ||
|| **LINE** | Open Channel ID. | ||
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
    "text": "Sergey \"Pokoiev\":\n Test message 55"
},
"chat": {
    "id": "2"
}
}
```
| ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}