# Deleting an Open Line OnImConnectorLineDelete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- types and requiredness of parameters are not specified
  
{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

Deleting an open line. You need to remove your connected connectors.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

#|
|| **Parameter** | **Description** | **Available since** ||
|| **LINE_ID** | ID of the open line to be deleted. | ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}