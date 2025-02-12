# Overview of Methods

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- No content available
- Replace the link

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

Configurable deals are a type of deal that can only be created from the application. For this type, you can customize the appearance of the deal card and its functionality. Methods for working with custom deal types in the [timeline](../../index.md)

#|
|| **Method** | **Description** ||
|| [crm.activity.configurable.add](./crm-activity-configurable-add.md) | Adds a new configurable deal to the timeline ||
|| [crm.activity.configurable.update](./crm-activity-configurable-update.md) | Updates a configurable deal ||
|| [crm.activity.configurable.get](./crm-activity-configurable-get.md) | Retrieves information about the deal ||
|| [crm.activity.delete](../crm-activity-delete.md) | Deletes a configurable deal by its identifier ||
|| [crm.activity.list](../crm-activity-list.md) | Retrieves a list of all configurable deals for a CRM entity filtered by `PROVIDER_ID` = `CONFIGURABLE_REST_APP` ||
|#

{% note warning %}

The methods `crm.activity.configurable.add`, `crm.activity.configurable.update`, `crm.activity.configurable.get` can only be called in the context of a [Rest application](https://helpdesk.bitrix24.com/examples/app.zip).

{% endnote %}

## Additional

- [{#T}](./structure/layout.md)
- [{#T}](./badges/index.md)