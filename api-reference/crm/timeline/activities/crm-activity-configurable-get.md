# Get Configurable Deal by ID crm.activity.configurable.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- required parameters are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.configurable.get` returns information about a deal.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **ID**
[`number`](../../../data-types.md) | Identifier of the deal | ||
|#

The method returns an associative array with the key **activity**, which will contain:

- **id** - identifier of the deal
- **ownerTypeId** - identifier of the entity type to which the deal is linked.
- **ownerId** - identifier of the main entity element to which the deal is linked.
- **fields** - associative array of field values of the deal
- [**layout**](./structure/layout.md) - associative array of a special structure describing the appearance of the deal in the timeline.