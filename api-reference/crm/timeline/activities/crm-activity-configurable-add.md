# Add Configurable CRM Activity

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Required parameters are not specified
- Examples are missing
- Success response is absent
- Error response is absent

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.configurable.add` adds a configurable deal. The method can only be called in the context of a Rest application.

## Parameters

#|
|| **Parameter** | **Description** | **Available from** ||
|| **ownerTypeId**
[`number`](../../../data-types.md) | Identifier of the entity type to which the created deal will be linked. | ||
|| **ownerId**
[`number`](../../../data-types.md) | Identifier of the element of this entity to which the created deal will be linked. | ||
|| **fields**
[`array`](../../../data-types.md) | Associative array of deal fields. | ||
|| **layout**
[`LayoutDto`](./structure/layout.md) | Associative array of a special structure describing the appearance of the deal in the timeline. | ||
|#