# OnImConnectorMessageDelete Event

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- types and requiredness of parameters are not specified.

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

This event is triggered when a message is deleted from the open line.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

#|
|| **Parameter** | **Description** | **Available since** ||
|| **CONNECTOR**| Connector ID (used to verify if this event belongs to the verifier). | ||
|| **LINE** | Open line ID. | ||
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