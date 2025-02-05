# Activate Connector imconnector.activate

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

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

The method sets, activates, or deactivates a channel for a specific open line.

{% note info "Note" %}

If you want the connector to appear in the general list of connectors in the widget on the website, you need to use the method [imconnector.connector.data.set](./imconnector-connector-data-set.md). [Example usage](../../../tutorials/openlines/example-connector.md).

{% endnote %}

## Parameters

#|
|| **Parameter** | **Description** | **Version** ||
|| **CONNECTOR**
[`unknown`](../../data-types.md) | ID of the connector (which was specified during the handler registration). | ||
|| **LINE**
[`unknown`](../../data-types.md) | ID of the open line. | ||
|| **ACTIVE**
[`unknown`](../../data-types.md) | 1 or 0. Activate or deactivate. | ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}