# Change Text in the Center of the Call Card from the Application CallCardSetStatusText

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

The `CallCardSetStatusText` method allows the application to change the text in the center of the call card.

When called, it requires an object with the property statusText. Nothing is passed to the callback function.

## Example

```js
BX24.placement.bindEvent('BackgroundCallCard::initialized', event => {
    BX24.placement.call('CallCardSetStatusText', { statusText: 'hello world!' }, () => {
        // some code
    })
});
```

{% include [Footnote on examples](../../../../_includes/examples.md) %}