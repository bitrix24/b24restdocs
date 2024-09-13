# Send Messages to Bitrix24 imconnector.send.messages

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter type is not specified
- parameter mandatory status is not specified
- examples in other languages are missing

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

Method for sending messages in Open Lines.

## Parameters

#|
|| **Parameter** | **Description** | **From version** ||
|| **CONNECTOR**
[`unknown`](../../data-types.md) | Connector ID (as specified during handler registration). | ||
|| **LINE**
[`unknown`](../../data-types.md) | Open line ID. | ||
|| **MESSAGES**
[`unknown`](../../data-types.md) | Array of messages, where messages are described in the following format: 

```
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
                'email', //email
                'phone', //phone
                'skip_phone_validate' => 'Y', //In value 'Y' allows skipping validation 
                                              //of the user's phone number. By default         
        ),
        //Array describing the message
        'message' => array(
            'id', //Message ID in the external system.*
            'date', //Message time in timestamp format *
            'disable_crm' => 'Y' ,//disable chat tracker (CRM tracker)
            'text', //Message text. Either the text or files element must be specified. 
                    //Allowed formatting (BB codes) is described 
                    //here: https://apidocs.bitrix24.com/api-reference/chats/messages/index.html
            'files' => array(//Array of file descriptions, where each file is described 
                             //by an array with a link available to the account
                array('url' => 'Link to file'),
                array('url' => 'Link to file'),
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
The format of the transmitted file has no restrictions. In chat, attachments in messages can be formatted as images for types: jpe, jpg, jpeg, png, webp, gif, bmp.

Messages can be sent on behalf of the manager by specifying user_id in the message array.
| ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

{% note info "Note" %}

The **skip_phone_validate** parameter in the user structure is recommended to be used only in exceptional cases. This parameter is a necessary measure to overcome the limitations of the phone number validator.

{% endnote %}

## Errors When Calling the Method and Their Causes

#|
|| **Code** | **Error Message** | **Explanation** ||
|| **WRONG_AUTH_TYPE** | Current authorization type is denied for this method | Incorrect authorization type. OAuth type is required. ||
|| **CONNECTOR** | Argument 'CONNECTOR' is null or empty | Required parameter 'CONNECTOR' is not specified in the request. ||
|| **LINE** | Argument 'LINE' is null or empty | Required parameter 'LINE' is not specified in the request. ||
|| **MESSAGES** | Argument 'MESSAGES' is null or empty | Required parameter 'MESSAGES' is not specified in the request. ||
|| **MESSAGES** | The value of an argument 'MESSAGES' must be of type array | The parameter value is not an array. ||
|| **IMCONNECTOR_NO_CORRECT_PROVIDER** | Could not find a suitable provider for the connector | Incorrect value in the 'CONNECTOR' parameter. ||
|| **IMCONNECTOR_COULD_NOT_GET_PROVIDER_OBJECT** | Could not get the provider object | Incorrect value in the 'CONNECTOR' parameter. ||
|| **IMCONNECTOR_NOT_SPECIFIED_CORRECT_COMMAND** | Correct command not specified | Something incredible. The developer made a mistake somewhere. ||
|| **IMCONNECTOR_NOT_SPECIFIED_CORRECT_CONNECTOR** | Connector not specified | Incorrect value in the 'CONNECTOR' parameter. ||
|| **NOT_ACTIVE_LINE** | Line with such ID is inactive or does not exist | The line has been deleted or disabled in the account. ||
|| **PROVIDER_UNSUPPORTED_TYPE_INCOMING_MESSAGE** | Unsupported type of incoming message from the server | Incorrect value in the 'type_message' parameter, if it was passed. ||
|| **IMCONNECTOR_NOT_ALL_THE_REQUIRED_DATA** | Not all required data has been provided | Empty or incorrect value in the 'user' parameter. ||
|| **CONNECTOR_PROXY_NO_ADD_USER** | Could not create or get the system user mapped to the remote messenger user | To operate the open line chat, a special technical user must be added to the account, marked as a user for the messenger connector, and under which it is impossible to authorize. ||
|| **CONNECTOR_PROXY_NO_USER_IM** | Messenger user ID not obtained | Incorrect value in the 'id' field in the 'user' parameter. This is a consequence of the previous error. ||
|| **IMCONNECTOR_NOT_ALL_THE_REQUIRED_DATA** | Not all required data has been provided | Incorrect value in the 'text' or 'files' field in the 'message' parameter. Some data for sending the message has not been provided. ||
|| **100** | The MESSAGES parameter must be an array of messages (arrays) | The value of the 'MESSAGES' parameter must be an array of messages. ||
|| **100** | The incorrect structure of a message inside the MESSAGES parameter. | Incorrect structure of messages. ||
|#