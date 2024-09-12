# Close the call card from the CallCardClose application

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The `CallCardClose` method allows the application to close the call card.

{% note warning %}

Attention! After calling this method, the call card will be closed, and no further operations can be performed on it.

{% endnote %}

## Example

```js
BX24.placement.bindEvent('BackgroundCallCard::initialized', event => {
    BX24.placement.call('CallCardClose', {}, () => {
        // some code
    })
});
```

{% include [Footnote about examples](../../../../_includes/examples.md) %}