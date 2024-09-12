# When selecting an operator to whom the current operator wants to transfer the call BackgroundCallCard::transferButtonClick

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing

{% endnote %}

{% endif %}

> Scope: [`telephony`](../../../../scopes/permissions.md)
>
> Who can subscribe: `any user`

The event `BackgroundCallCard::transferButtonClick` occurs when selecting an operator to whom the current operator wants to transfer the call. The callback function receives an object with the properties `phoneNumber` — the number of the current call and `target` — the identifier of the Bitrix24 user.