# OnImConnectorMessageDelete Event

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Types and required parameters are not specified.

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

Event for message deletion from Open Channels.

#| 
|| **Parameter** | **Description** | **Available since** ||
|| **CONNECTOR** | Connector ID (used to check if this event belongs to the verifier). | ||
|| **LINE** | Open Channel ID. | ||
|| **DATA** | Array of messages, where each message is described by an array of the following format:


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