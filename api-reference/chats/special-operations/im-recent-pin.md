# Pin a chat at the top of the im.recent list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.recent.pin` pins a conversation at the top of the chat list.

## Parameters

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **DIALOG_ID^*^**
[`unknown`](../../data-types.md) | `chat17`
or
`256` | Identifier of the dialog. Format:
- **chatXXX** – chat of the recipient, if the message is for a chat
- **XXX** – identifier of the recipient, if the message is for a private dialog | 19 ||
|| **PIN**
[`unknown`](../../data-types.md) | `Y` | Pin or unpin the dialog | 19 ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

- If the parameter `PIN = N` is specified, the pinned dialog will be unpinned.

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```js
    BX24.callMethod(
        'im.recent.pin',
        {
            'DIALOG_ID': 'chat17',
            'PIN': 'Y'
        },
        function(result){
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP

    {% include [Explanation about restCommand](../_includes/rest-command.md) %}

    ```php
    $result = restCommand(
        'im.recent.pin',
        Array(
            'DIALOG_ID' => 'chat17',
            'PIN' => 'Y'
        ),
        $_REQUEST[
            "auth"
        ]
    );    
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response on success

```json
{
    "result": true
}
```

## Response on error

```json
{
    "error": "DIALOG_ID_EMPTY",
    "error_description": "Dialog ID can't be empty"
}
```

### Description of keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible error codes

#|
|| **Code** | **Description** ||
|| **DIALOG_ID_EMPTY** | Dialog ID was not provided. ||
|#