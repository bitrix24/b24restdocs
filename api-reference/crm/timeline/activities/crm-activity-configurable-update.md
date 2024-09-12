# Update Configurable CRM Activity

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

The method `crm.activity.configurable.update` makes changes to a configurable deal. Updating the deal is only possible in the context of the Rest application that created it.

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **id**
[`number`](../../../data-types.md) | Identifier of the deal. | ||
|| **fields**
[`array`](../../../data-types.md) | Associative array of deal fields. | ||
|| **layout**
[LayoutDto](./structure/layout.md) | Associative array of a special structure describing the appearance of the deal in the timeline. | || 
|#