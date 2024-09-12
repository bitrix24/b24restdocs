# Get the list of badges crm.activity.badge.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.badge.list` retrieves a list of available badges. It will return an array containing a list of all registered badges. Each element of the array contains the badge fields.

## Parameters

No parameters.