# Activate Connector imconnector.activate

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent
- these files were not in the structure, but there are links to them in the Note: imconnector.connector.data.set and cases/example_connector_chat

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method sets, activates, or deactivates a channel for a specific open line.

{% note info "Note" %}

If you want the connector to be displayed in the general list of connectors in the widget on the site, you need to use the method [imconnector.connector.data.set](.). [Example of usage](.).

{% endnote %}

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **CONNECTOR**
[`unknown`](../../data-types.md) | Connector ID (as specified during handler registration). | ||
|| **LINE**
[`unknown`](../../data-types.md) | Open line ID. | ||
|| **ACTIVE**
[`unknown`](../../data-types.md) | 1 or 0. Activate or deactivate. | ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}