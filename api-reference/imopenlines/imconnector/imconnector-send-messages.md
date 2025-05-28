# Send messages to Bitrix24 imconnector.send.messages

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- no examples

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method sends messages to an open line. The method parameters use values from the external system — these are the user ID, chat ID, links to the chat, and its names in the application that [registered](./imconnector-register.md) the connector.

The created chat ID and open line dialog ID values will be returned as a result of the method execution.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONNECTOR**
[`unknown`](../../data-types.md) | Identifier `ID` of the connector specified during handler registration ||
|| **LINE**
[`unknown`](../../data-types.md) | Identifier `ID` of the open line ||
|| **MESSAGES**
[`unknown`](../../data-types.md) | Array of messages. Each message is described by a separate array, including information about the user, message, and chat.

```js
array(
    array(
        //Array describing the user
        'user' => array(
            'id',//User ID in the external system *
            'last_name',//Last name
            'name',//First name
            'picture' => array(
                'url'//Link to the user's avatar available for the account
            ),
            'url',//Link to the user's profile
            'sex',//Gender. Acceptable values are male and female
            'email', //email
            'phone', //phone
            'skip_phone_validate' => 'Y', //In value 'Y' allows to skip validation 
                                            //of the user's phone number. By default         
        ),
        //Array describing the message
        'message' => array(
            'id', //Message ID in the external system.*
            'date', //Message time in timestamp format *
            'disable_crm' => 'Y' ,//disable chat tracker (CRM tracker)
            'text', //Message text. Either text or files must be specified. 
                    //Allowed formatting (BB codes) is described 
                    //here: https://apidocs.bitrix24.com/api-reference/chats/messages/index.html
            'files' => array(//Array of file descriptions, where each file is described 
                             //by an array with a link available to the account
                array('url' => 'Link to file', 'name' => 'File name'),
                array('url' => 'Link to file', 'name' => 'File name'),
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
In the `user` array, the fields `name` and `last_name` can contain letters, spaces, hyphens, and apostrophes. Numbers and other special characters are not allowed. The maximum length for `name` and `last_name` is 25 characters. The language and case can be any.

The format of the transmitted file has no restrictions. In the chat, attachments in messages can be formatted as images for types: jpe, jpg, jpeg, png, webp, gif, bmp.

Messages can be sent on behalf of the manager by specifying `user_id` in the message array.
||
|#

{% note info "Note" %}

The `skip_phone_validate` parameter in the user structure is recommended to be used only in exceptional cases. This parameter is a forced measure to overcome the limitations of the phone number validator.

{% endnote %}

## Response Handling

HTTP status: **200**

```json
{
    "SUCCESS": true,
    "DATA": {
        "RESULT": [
            {
                "user": "1670",
                "message": {
                    "id": "1",
                    "date": {},
                    "disable_crm": "true",
                    "text": "test message"
                },
                "chat": {
                    "id": "1",
                    "name": "test_chat",
                    "description": "Link to the original post: example.com"
                },
                "extra": {
                    "skip_phone_validate": "Y"
                },
                "SUCCESS": true,
                "session": {
                    "ID": "3267",
                    "CHAT_ID": "9853"
                }
            }
        ]
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **SUCCESS**
[`boolean`](../../data-types.md) | Returns `true` in case of successful sending ||
|| **DATA**
[`object`](../../data-types.md) | Contains the object [`RESULT`](#result) with information about the sent messages ||
|#

#### Object RESULT {#result}

#|
|| **Name**
`type` | **Description** ||
|| **user**
[`integer`](../../data-types.md) | User identifier in the external system ||
|| **message**
[`object`](../../data-types.md) | Object with information about the message:
- `id` — message identifier in the external system
- `date` — message time in timestamp format
- `disable_crm` — whether the chat tracker is disabled
- `text` — message text
 ||
|| **chat**
[`object`](../../data-types.md) | Object with information about the chat:
- `id` — chat identifier in the external system
- `name` — chat name in the external system
- `description` — chat description
  ||
|| **extra**
[`object`](../../data-types.md) | Object with information about additional settings:
- `skip_phone_validate` — whether the phone number validation was skipped ||
|| **SUCCESS**
[`boolean`](../../data-types.md) | Returns `true` in case of successful sending ||
|| **session**
[`object`](../../data-types.md) | Object with information about the open line dialog
- `ID` — session identifier
- `CHAT_ID` — chat identifier   ||
|#

## Error Handling

### Possible Error Codes

#|
|| **Code** | **Description** | **Explanation** ||
|| `WRONG_AUTH_TYPE` | Current authorization type is denied for this method | Incorrect authorization type. OAuth type is required ||
|| `CONNECTOR` | Argument 'CONNECTOR' is null or empty | Required parameter `CONNECTOR` is not specified in the request ||
|| `LINE` | Argument 'LINE' is null or empty | Required parameter `LINE` is not specified in the request ||
|| `MESSAGES` | Argument 'MESSAGES' is null or empty | Required parameter `MESSAGES` is not specified in the request ||
|| `MESSAGES` | The value of an argument 'MESSAGES' must be of type array | The parameter value is not an array. ||
|| `IMCONNECTOR_NO_CORRECT_PROVIDER` | Could not find a suitable provider for the connector | Incorrect value in the `CONNECTOR` parameter ||
|| `IMCONNECTOR_COULD_NOT_GET_PROVIDER_OBJECT` | Could not get the provider object | Incorrect value in the `CONNECTOR` parameter ||
|| `IMCONNECTOR_NOT_SPECIFIED_CORRECT_COMMAND` | Correct command not specified | Something incredible. This is where the developer made a mistake ||
|| `IMCONNECTOR_NOT_SPECIFIED_CORRECT_CONNECTOR` | Connector not specified | Incorrect value in the `CONNECTOR` parameter ||
|| `NOT_ACTIVE_LINE` | Line with this ID is inactive or does not exist | The line has been deleted or disabled on the account ||
|| `PROVIDER_UNSUPPORTED_TYPE_INCOMING_MESSAGE` | Unsupported type of incoming message from the server | Incorrect value in the `type_message` parameter, if it was passed ||
|| `IMCONNECTOR_NOT_ALL_THE_REQUIRED_DATA` | Not all required data was provided | Empty or incorrect value in the `user` parameter ||
|| `CONNECTOR_PROXY_NO_ADD_USER` | Could not create or get the system user associated with the remote messenger user | To work with open line chats, a special technical user must be added to the account, marked as a user for the messenger connector, and under which authorization is not possible ||
|| `CONNECTOR_PROXY_NO_USER_IM` | Messenger user ID not obtained | Incorrect value in the `id` field in the `user` parameter. This is a consequence of the previous error ||
|| `IMCONNECTOR_NOT_ALL_THE_REQUIRED_DATA` | Not all required data was provided | Incorrect value in the `text` or `files` field in the `message` parameter. Some data for sending the message was not provided ||
|| `100` | The MESSAGES parameter must be an array of messages (arrays) | The value of the `MESSAGES` parameter must be an array of messages ||
|| `100` | The incorrect structure of a message inside the MESSAGES parameter. | Incorrect structure of messages ||
|#