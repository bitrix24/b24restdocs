# On disconnecting the open channel OnImConnectorStatusDelete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- types and requiredness of parameters are not specified.

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

Triggers when a client disconnects a connected channel on a specific line. The data has already been removed from the account, and necessary actions must be taken on your end to disconnect.

#|
|| **Parameter** | **Description** | **Available since** ||
|| **CONNECTOR** | ID of the disconnected connector. | ||
|| **LINE** | ID of the removed open channel. | ||
|#

{% include [Parameter notes](../../../../_includes/required.md) %}