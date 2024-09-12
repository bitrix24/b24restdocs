# Delete Sent Messages imconnector.delete.messages

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter type is not specified
- parameter mandatory status is not specified
- examples are missing
- responses for success and error cases are not specified

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

Method for updating messages in OL.

## Parameters

#|
|| **Parameter** | **Description** | **Version** ||
|| **CONNECTOR**
[`unknown`](../../data-types.md) | Connector ID (as specified during handler registration). | ||
|| **LINE**
[`unknown`](../../data-types.md) | ID of the open line. | ||
|| **MESSAGES**
[`unknown`](../../data-types.md) | Array of messages, where messages are described in the following format: 

```js
array(
    array(
        //Array describing the user
        'user' => array(
            'id', //User ID in the external system *
        ),
        //Array describing the message
        'message' => array(
            'id', //Message ID in the external system.*
        ),
        //Array describing the chat
        'chat' => array(
            'id', //Chat ID in the external system *
        ),
    ),
    array(...)
);
```

| ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}