# Custom Connectors for Open Lines

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- from Sergey’s file: how it works, in what cases it will be useful, user scenario for connecting through the interface.

{% endnote %}

{% endif %}

Description of REST methods and events that work with connectors for external messengers. Permission **imopenlines**.

{% note info "Note" %}

REST methods in the **imconnector** scope in the current version do not support working through webhooks.

{% endnote %}

REST methods available when working with connectors for external messengers.

#|
|| **Method** | **Description** ||
|| [imconnector.register](imconnector-register.md) | Register a connector. ||
|| [imconnector.activate](imconnector-activate.md) | Activate a connector. ||
|| [imconnector.deactivate](imconnector-deactivate.md) | Deactivate a connector. ||
|| [imconnector.status](imconnector-status.md) | Get the status of a connector. ||
|| [imconnector.list](imconnector-list.md) | Retrieve a list of connectors. ||
|| [imconnector.unregister](imconnector-unregister.md) | Unregister a connector. ||
|| [imconnector.send.messages](imconnector-send-messages.md) | Send messages to Bitrix24. ||
|| [imconnector.update.messages](imconnector-update-messages.md) | Modify sent messages. ||
|| [imconnector.delete.messages](imconnector-delete-messages.md) | Delete sent messages. ||
|| [imconnector.send.status.delivery](imconnector-send-status-delivery.md) | Update status to “delivered”. ||
|| [imconnector.send.status.reading](imconnector-send-status-reading.md) | Update status to “read”. ||
|#