# Set "Like" for the imbot.message.like message

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent
- links to pages that have not yet been created are not specified

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.message.like` sets a "Like" label on a message.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **BOT_ID**
[`unknown`](../../data-types.md) | `39` | Identifier of the chatbot making the request; can be omitted if there is only one chatbot | ||
|| **MESSAGE_ID**
[`unknown`](../../data-types.md) | `1` | Identifier of the message (any message sent in private dialogs or in group chats where the chatbot is present) | ||
|| **ACTION**
[`unknown`](../../data-types.md) | `'auto'` | Action related to the method:
- plus - will set the "Like" label;
- minus - will remove the "Like" label;
- auto - will automatically determine whether to set or remove the label.
If not specified, it will operate in auto mode | ||
|#

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'imbot.message.like',
        Array(
            'BOT_ID' => 39,
            'MESSAGE_ID' => 1,
            'ACTION' => 'auto'
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response in case of success

`true`.

## Response in case of error

error

### Possible error codes

#|
|| **Code** | **Description** ||
|| **BOT_ID_ERROR** | Chatbot not found. ||
|| **APP_ID_ERROR** | Chatbot does not belong to this application. Only chatbots installed within the application can be used. ||
|| **MESSAGE_ID_ERROR** | Message identifier not provided. ||
|| **WITHOUT_CHANGES** | The "Like" status did not change after the call. ||
|#