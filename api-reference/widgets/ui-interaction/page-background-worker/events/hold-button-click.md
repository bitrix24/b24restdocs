# On Clicking the Hold Call Button BackgroundCallCard::holdButtonClick

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing

{% endnote %}

{% endif %}

> Scope: [`telephony`](../../../../scopes/permissions.md)
>
> Who can subscribe: `any user`

The event `BackgroundCallCard::holdButtonClick` occurs when the hold call button is clicked. A boolean value is passed to the callback function: true - hold is expected to be activated, false - hold is expected to be deactivated.