# Transfer a dialogue to another operator or line imopenlines.operator.transfer

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

This method allows the current operator to transfer a dialogue to another operator/line.

## Method Parameters

#|
|| **Name**
`Type` | **Description** | **Available since** ||
|| **CHAT_ID**
[`unknown`](../../../data-types.md) | Identifier of the chat that the current operator is ending | ||
|| **TRANSFER_ID**
[`unknown`](../../../data-types.md) | Identifier of the entity to which the dialogue is being transferred. If the dialogue is to be transferred to an operator, the operator's ID is passed as the value. If to a line — the code in the format "queue#line ID#" | ||
|#