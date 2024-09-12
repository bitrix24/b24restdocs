# Get the status of the connector imconnector.status

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent
  
{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns the current state of the connector.

## Parameters

#|
|| **Parameter** | **Description** | **Available from** ||
|| **LINE**
[`unknown`](../../data-types.md) | ID of the open line. | ||
|| **CONNECTOR**
[`unknown`](../../data-types.md) | ID of the connector (which was specified during the handler registration). | ||
|| **ERROR**
[`unknown`](../../data-types.md) | Was there an error in the channel. | ||
|| **CONFIGURED**
[`unknown`](../../data-types.md) | Is the channel configured. | ||
|| **STATUS**
[`unknown`](../../data-types.md) | Current status of the channel (can it transmit data (configured and does not contain an error)). | ||
|#

{% include [Parameter notes](../../../_includes/required.md) %}