# Change the state of the call card interface from the CallCardSetUiState application

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

The `CallCardSetUiState` method allows the application to change the state of the call card interface.

The application must pass an object with the property uiState to the method, where the value should be one of those obtained from the getListUiStates method. No data is passed to the callback function.

## Example

```js
BX24.placement.bindEvent('BackgroundCallCard::initialized', event => {
    BX24.placement.call('CallCardSetUiState', {uiState: 'connected'}, () => {
        // some code 
    })
});
```

{% include [Footnote about examples](../../../../_includes/examples.md) %}