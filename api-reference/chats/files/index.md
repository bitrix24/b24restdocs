# Get the file storage folder of the specified chat im.disk.folder.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- revisions needed for writing standards
- parameter types not specified
- examples missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.disk.folder.get` retrieves information about the file storage folder for a chat.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **CHAT_ID^*^**
[`unknown`](../../data-types.md) | `17` | Identifier of the chat | 18 ||
|#

{% include [Parameter notes](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```js
    BX24.callMethod(
        'im.disk.folder.get', {
            'CHAT_ID': 17,
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
        'im.disk.folder.get',
        Array(
            'CHAT_ID' => 17,
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Examples notes](../../../_includes/examples.md) %}

## Successful response

```json
{
    "result": {
        "ID": 127
    }
}
```

## Error response

```json
{
    "error": "CHAT_ID_EMPTY",
    "error_description": "Chat ID can't be empty"
}
```

### Description of keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

## Possible error codes

#|
|| **Code** | **Description** ||
|| **CHAT_ID_EMPTY** | Chat identifier not provided ||
|| **ACCESS_ERROR** | The current user does not have access permission to the dialog ||
|| **INTERNAL_ERROR** | Server error, please contact technical support ||
|#