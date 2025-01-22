# Remove the imbot.command.unregister command

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- required parameters are not indicated
- not all parameters have examples in the table
- examples are missing
- response on success is missing
- response on error is missing
- links to pages that have not yet been created are not specified

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.command.unregister` removes the command handling.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **COMMAND_ID**
[`unknown`](../../data-types.md) | `13` | Identifier of the command to be removed | ||
|| **CLIENT_ID**
[`unknown`](../../data-types.md) | `''` | String identifier of the chatbot, used only in Webhook mode | ||
|#

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'imbot.command.unregister',
        Array(
            'COMMAND_ID' => 13,
            'CLIENT_ID' => '',
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% note warning %}

To process the command, the application must handle the command addition event [ONIMCOMMANDADD](./events/on-im-command-add.md).

{% endnote %}

## Response on success

`true`

## Response on error

error

### Possible error codes

#|
|| **Code** | **Description** ||
|| **COMMAND_ID_ERROR** | Command not found. ||
|| **APP_ID_ERROR** | The chatbot does not belong to this application. You can only work with chatbots installed within the application. ||
|| **WRONG_REQUEST** | Something went wrong. ||
|#