# Event for Creating a Deal onCrmDealAdd

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter type is not specified
- examples are missing (in other languages)

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `onCrmDealAdd` event is triggered when a deal is created.

#|
|| **Parameter** | **Description** ||
|| **FIELDS** | The array contains the ID field with the value of the created deal's identifier. ||
|#

## Example

You can subscribe to the method via REST like this:

```php
CRest::call('event.bind',
    [
        'event' => 'onCrmDealAdd',
        'handler' => 'https://example.com/handler.php'
    ]
);
```

{% include [Note on Examples](../../../../_includes/examples.md) %}