# Disable Notifications from Chat im.chat.mute

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.chat.mute` disables notifications in the chat.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **CHAT_ID^*^**
[`unknown`](../../data-types.md) | `17` | Identifier of the chat | 19 ||
|| **MUTE^*^**
[`unknown`](../../data-types.md) | `Y` | Options for the MUTE key: `Y` to disable notifications and `N` to enable. | 19 ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```javascript
    BX24.callMethod(
        'im.chat.mute',
        {
            'CHAT_ID': 17,
            'MUTE': 'Y'
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
        'im.chat.mute',
        Array(
            'CHAT_ID' => 17,
            'MUTE' => 'Y'
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Examples Note](../../../_includes/examples.md) %}

## Success Response

```json
{
    "result": true
}
```

## Error Response

```json
{
    "error": "CHAT_ID_EMPTY",
    "error_description": "Chat ID can't be empty"
}
```

### Key Descriptions

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **CHAT_ID_EMPTY** | Chat identifier not provided ||
|#