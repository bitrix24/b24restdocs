# Finish Your Dialogue imopenlines.operator.finish

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method ends the dialogue by the current operator.

## Method Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **CHAT_ID**
[`unknown`](../../../data-types.md) | The identifier of the chat that the current operator is ending | ||
|#