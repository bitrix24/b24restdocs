# Take the dialog from the current operator imopenlines.session.intercept

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent
- from Sergei's file: mention that this is done for the user whose tokens are used to call the method, mention alternative methods pin*, and the method from the operators section

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method allows the current operator to take a dialog that already has another operator.

## Method Parameters

#|
|| **Name**
`Type` | **Description** | **Available from** ||
|| **CHAT_ID**
[`unknown`](../../../data-types.md) | The identifier of the chat that the current operator is taking | ||
|#