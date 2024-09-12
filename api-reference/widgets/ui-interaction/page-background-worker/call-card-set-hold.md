# Set a Call on Hold from the CallCardSetHold Application

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

The `CallCardSetHold` method allows the application to place a call on hold.

The application must pass an object with the property held to the method, which contains a boolean value: true - to enable hold, false - to disable hold. No data is passed to the callback function.

## Example

```js
BX24.placement.bindEvent('BackgroundCallCard::initialized', event => {
    BX24.placement.call('CallCardSetHold', { held: true }, () => {
        // some code
    });
});
```

{% include [Note on examples](../../../../_includes/examples.md) %}