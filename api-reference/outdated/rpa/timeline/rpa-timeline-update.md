# Update Timeline Entry rpa.timeline.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.timeline.update` will update the timeline entry with the identifier id.

This method allows you to change only the fields title and description.

#|
|| **Parameter** / **Type** | **Description** ||
|| **id** 
[`number`](../../../data-types.md) | Identifier of the entry. ||
|| **fields** 
[`array`](../../../data-types.md) | Entry fields. ||
|#

## Fields Parameters

#|
|| **Parameter** | **Description** ||
|| **title** | Title of the entry. ||
|| **description** | Description of the entry (HTML can be used). ||
|#

{% note warning %}

This method allows you to modify only those entries that were created by the same user and through the application.

{% endnote %}