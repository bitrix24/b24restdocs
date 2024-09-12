# Reference Tables

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- what types exist, where they are used, which are read-only
  
{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [crm.status.add](./crm-status-add.md) | Creates a new item in the specified reference table. ||
|| [crm.status.delete](./crm-status-delete.md) | Deletes an item from the reference table. ||
|| [crm.status.entity.items](./crm-status-entity-items.md) | Returns items from the reference table by its symbolic identifier. ||
|| [crm.status.entity.types](./crm-status-entity-types.md) | Returns descriptions of reference table types. ||
|| [crm.status.fields](./crm-status-fields.md) | Returns descriptions of reference table fields. ||
|| [crm.status.get](./crm-status-get.md) | Returns an item from the reference table by its identifier. ||
|| [crm.status.list](./crm-status-list.md) | Returns a list of items from the reference table based on a filter. ||
|| [crm.status.update](./crm-status-update.md) | Updates an existing item in the reference table. ||
|#