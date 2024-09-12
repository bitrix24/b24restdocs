# Get a List of Available Call Card UI States CallCardGetListUiStates

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The `CallCardGetListUiStates` method allows you to retrieve a list of available call card UI states.

When called, an object containing all possible call card UI states is passed to the callback function.

## Example

```js
BX24.placement.bindEvent('BackgroundCallCard::initialized', event => {
    BX24.placement.call('CallCardGetListUiStates', data => {
        // some code
    })
});
```

{% include [Footnote on examples](../../../../_includes/examples.md) %}