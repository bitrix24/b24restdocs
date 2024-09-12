# Disable Operator Microphone from the CallCardSetMute Application

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

The `CallCardSetMute` method allows the application to disable the operator's microphone.

The application must pass an object to the method with the property muted, which contains a boolean value: true - the microphone should be off, false - on. No data is passed to the callback function.

## Example

```js
BX24.placement.bindEvent('BackgroundCallCard::initialized', event => {
    BX24.placement.call('CallCardSetMute', { muted: true }, () => {
// some code
    });
});
```

{% include [Note on examples](../../../../_includes/examples.md) %}