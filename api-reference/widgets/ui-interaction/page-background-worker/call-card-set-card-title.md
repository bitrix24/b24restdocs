# Change the title of the call card from the CallCardSetCardTitle application

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

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

The `CallCardSetCardTitle` method allows you to change the title of the call card from the application.

When called, you need to pass an object with the property title. Nothing is passed to the callback function.

## Example

```js
BX24.placement.bindEvent('BackgroundCallCard::initialized', event => {
    BX24.placement.call('CallCardSetCardTitle', { title: 'hello world!' }, () => {
        // some code
    })
});
```

{% include [Example note](../../../../_includes/examples.md) %}