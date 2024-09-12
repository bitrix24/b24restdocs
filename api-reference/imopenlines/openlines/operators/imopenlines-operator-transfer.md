# Transfer a conversation to another operator or line imopenlines.operator.transfer

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

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method allows the current operator to transfer a conversation to another operator/line.

## Method Parameters

#|
|| **Name**
`Type` | **Description** | **Available since** ||
|| **CHAT_ID**
[`unknown`](../../../data-types.md) | The identifier of the chat that the current operator is ending | ||
|| **TRANSFER_ID**
[`unknown`](../../../data-types.md) | The identifier of the entity to which the conversation is being transferred. If transferring to an operator, the operator's ID is provided as the value. If to a line — the code format is "queue#LINE ID#" | || 
|#