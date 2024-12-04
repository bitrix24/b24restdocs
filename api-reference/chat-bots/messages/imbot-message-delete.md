# Delete Message imbot.message.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent
- links to pages that have not yet been created are not specified

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.message.delete` deletes a message from the chatbot.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **BOT_ID^*^**
[`unknown`](../../data-types.md) | `39` | Identifier of the chatbot from which the request is made; can be omitted if there is only one bot | ||
|| **MESSAGE_ID^*^**
[`unknown`](../../data-types.md) | `1` | Identifier of the message | ||
|| **COMPLETE**
[`unknown`](../../data-types.md) | `'N'` | If the message needs to be deleted completely, without traces, then 'Y' must be specified | ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'imbot.message.delete',
        Array(
            'BOT_ID' => 39,
            'MESSAGE_ID' => 1,
            'COMPLETE' => 'N',
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Success Response

`true`.

## Error Response

error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **BOT_ID_ERROR** | Chatbot not found. ||
|| **APP_ID_ERROR** | The chatbot does not belong to this application. You can only work with chatbots installed within the application. ||
|| **MESSAGE_ID_ERROR** | Message identifier not provided. ||
|| **CANT_EDIT_MESSAGE** | You do not have access to this message. ||
|#