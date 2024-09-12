# Update Sent Messages imconnector.update.messages

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
|| **Parameter** | **Description** | **Available since** ||
|| **CONNECTOR**
[`unknown`](../../data-types.md) | Connector ID (as specified during handler registration). | ||
|| **LINE**
[`unknown`](../../data-types.md) | Open line ID. | ||
|| **MESSAGES**
[`unknown`](../../data-types.md) | Array of messages, where messages are described by an array in the following format: 

```js
array(
    array(
        //Array describing the user
        'user' => array(
            'id',//User ID in the external system *
            'last_name',//Last name
            'name',//First name
            'picture' =>
                array(
                    'url'//Link to the user's avatar available for the account
                ),
            'url',//Link to the user's profile
            'sex',//Gender. Acceptable values are male and female
        ),
        //Array describing the message
        'message' => array(
            'id', //Message ID in the external system.*
            'date', //Message time in timestamp format *
            'text', //Message text. Either the text or files element must be specified. 
                    //Allowed formatting (BB codes) is described 
                    //here: /api-reference/chats/messages/index.html
            'files' => array(//Array of file descriptions, where each file is described 
                            //by an array with a link available for the account
                array('url'),
                array('url'),
                ...
            )
        ),
        //Array describing the chat
        'chat' => array(
            'id',//Chat ID in the external system *
            'name', //Chat name in the external system
            'url', //Link to the chat in the external system
        ),
    ),
    array(...),
);
```

| ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}