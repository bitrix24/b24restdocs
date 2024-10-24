# Delete Sent Messages imconnector.delete.messages

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter type is not specified
- parameter requirement is not specified
- examples are missing
- responses in case of success and error are not specified

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

Method for updating messages in OL.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **CONNECTOR**
[`unknown`](../../data-types.md) | Connector ID (which was specified during the handler registration). | ||
|| **LINE**
[`unknown`](../../data-types.md) | Open line ID. | ||
|| **MESSAGES**
[`unknown`](../../data-types.md) | Array of messages, where messages are described by an array of the following format: 

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

{% include [Notes on parameters](../../../_includes/required.md) %}